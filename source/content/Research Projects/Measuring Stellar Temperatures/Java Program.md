Below is the program I coded to automate the process. I am not a great coder; it's been a good few years since comp-sci class, but this suited my needs well enough. I was not concerned with optimization or clean code. Anyway, as outlined in [[Overview]], this exact program will likely not work for you as it is optimized for my equipment. If you want to use this tool, treat this as a template/guide.

```java
import java.io.*;
import java.text.*;
import java.util.Scanner;
import java.io.IOException;
import java.lang.Math;

public class StarTemperature{
  
  // Define some constants for the math (planck's, speed of light, etc)
  public static final double RED_WAVELENGTH = 7.0*(Math.pow(10,-7));
  public static final double GREEN_WAVELENGTH = 5.50*(Math.pow(10,-7));
  public static final double BLUE_WAVELENGTH = 4.78*(Math.pow(10,-7));
  public static final double PLANCKH = 6.626*(Math.pow(10,-34)); 
  public static final double C = 3*(Math.pow(10,8));
  public static final double KB = 1.381*(Math.pow(10,-23));
  
  public static void main(String[] args)throws IOException{
    
    File file = new File("<FilePath\File.dat>");
    Scanner lengthScanner = new Scanner(file);
    
    //get # of rows in file to determine how long each array should be
    int fileLength = 0;
    while(lengthScanner.hasNext()){
      lengthScanner.nextLine();
      fileLength++;    
    }
    lengthScanner.close();
    
    //Make the arrays
    fileLength -= 1;
    Double[] x = new Double[fileLength];
    Double[] r = new Double[fileLength];
    Double[] g = new Double[fileLength];
    Double[] b = new Double[fileLength];
    
    Scanner sc = new Scanner(file);
    sc.nextLine(); // skip over the first line that says "#x R G B"
    
    //Read the dat file again and populate the arrays
    for(int i = 0; i<fileLength; i++){      
      String line = sc.nextLine();
      String[] separatedLine = line.split(" ");
      x[i] = Double.parseDouble(separatedLine[0]);
      r[i] = Double.parseDouble(separatedLine[1]);
      g[i] = Double.parseDouble(separatedLine[2]);
      b[i] = Double.parseDouble(separatedLine[3]);   
    }
    sc.close();
    
    //Generate smoothed graphs via convolution. Kernel size 1 seems to yield good results
    Double[] rSmoothed = Convolution(r, fileLength, 1);
    Double[] gSmoothed = Convolution(g, fileLength, 1);
    Double[] bSmoothed = Convolution(b, fileLength, 1);
    
    
    double maxR = InputCalculation(DetectPOIs(x, rSmoothed, Derivative(x, rSmoothed)));
    double maxG = InputCalculation(DetectPOIs(x, gSmoothed, Derivative(x, gSmoothed)));
    double maxB = InputCalculation(DetectPOIs(x, bSmoothed, Derivative(x, bSmoothed)));
    
//    for(int i = 0; i < fileLength; i++){
//     System.out.println(bSmoothed[i]);
//    }
    
    System.out.println("R: " + maxR);
    System.out.println("G: " + maxG);
    System.out.println("B: " + maxB);
    
    int temperature = FindTemperature(maxR, maxG, maxB);
    System.out.println("Temperature of star is approximately " + temperature + " Kelvin.");
    System.out.println("Difference: " + DifferenceCalculation(temperature, maxR, maxG, maxB));
  }
  
  //-------------------------------------- Methods -----------------------------------------------
  
  // Simple Gaussian Convolution to smooth out data for finding local min/max. Adjust kernel size for strength  ----------------------------
  public static Double[] Convolution(Double[] rawInput, int length, int kernel){
    Double[] smoothed = new Double[length];
    
    for(int i=0; i<length; i++){
      double avg = 0;
      int k = 0;
      int clipOccurences = 0;
      
      do{
        if((i+kernel-k)>=0 && (i+kernel-k) < length){
          avg += (rawInput[i+kernel-k]);  
        }else{
          clipOccurences++;
        }
        k++;
      }while(k<=2*kernel);
      avg = avg/(k-clipOccurences);
      smoothed[i] = avg;
    }
    return smoothed;
  }
  
  // Crude derivative via secant between each adjacent point ----------------------------
  public static Double[] Derivative(Double[] x, Double[] y){
    Double[] derivative = new Double[y.length-1];
    
    for(int i = 0; i<y.length-1; i++){
      derivative[i] = (y[i+1]-y[i])/(x[i+1]-x[i]);
    }
    return derivative; 
  }
  
  // automatically detect the points of interest (peak, tough, plateau) for R, G, and B ----------------------------
  public static Double[] DetectPOIs(Double[] x, Double[] y, Double[] yPrime){
    Double[] POIs = new Double[3];
    int size = yPrime.length;
    boolean isNormalized = true;
    
    //find the highest value (Plateau)
    double maxPlateau = 0.0;
    for(int i = 0; i<y.length-1; i++){
      if(y[i]>maxPlateau){
        maxPlateau = y[i];
      }
    }
    POIs[2] = maxPlateau;
    if(maxPlateau <= 1){
      isNormalized = true;
    }else{
      isNormalized = false;
    }
    
    // first identify where to start
    int startPoint = 0;
    for(int i = 5; i < size; i++){
      Double buffer = 0.0;
      for(int b = 0; b<5; b++){
        buffer += yPrime[i-b];
      }
      if(buffer < -0.1 && isNormalized == true){
        startPoint = i;
        i=size;
      }else if(buffer < -3000 && isNormalized == false){
        startPoint = i;
        i=size;
      }
    }
    
    //find local min (Trough)
    int minX = startPoint;
    while(!(yPrime[minX] < 0 && yPrime[minX+1] > 0)){
      minX++;
    }
    POIs[0] = y[minX+1];
    
    //find local max (Peak)
    int maxX = minX+1;
    while(!(yPrime[maxX] > 0 && yPrime[maxX+1] < 0)){
      maxX++;
    }
    POIs[1] = y[maxX+1];
    DecimalFormat df = new DecimalFormat("#.##");
    System.out.println("Max value detected: " +  df.format(maxPlateau) + "  Normalization set to " + isNormalized);
    
    return POIs;
  }
  
  // Combines the values found in DetectPOIs to calculate inputs to feed into the planck equation  ----------------------------
  public static Double InputCalculation(Double[] combo){
    double peak = combo[1];
    double trough = combo[0];
    double plateau = combo[2];
    
    double output = ((peak - trough)/plateau) + trough;
    
    return output;
  }
  
  // Planck function (floating point precision pls work with me here) ----------------------------
  public static Double PlanckFunction(Double lambda, int temperature){
    double planck = ((2*PLANCKH*Math.pow(C,2))/(Math.pow(lambda,5))) 
      * Math.pow((Math.exp((PLANCKH*C)/(lambda*KB*temperature)))-1,-1); 
    return planck;  
  }
  
  //Finds RMSE between predicted intensities and observed ----------------------------
  public static double DifferenceCalculation(int temperature, double MaxR, double MaxG, double MaxB){  
    //theoretical ratios (what the math predicts at this wavelength and temp)
    double thRG = (PlanckFunction(RED_WAVELENGTH,temperature)/PlanckFunction(GREEN_WAVELENGTH,temperature));
    double thGB = (PlanckFunction(GREEN_WAVELENGTH,temperature)/PlanckFunction(BLUE_WAVELENGTH,temperature));
    double thRB = (PlanckFunction(RED_WAVELENGTH,temperature)/PlanckFunction(BLUE_WAVELENGTH,temperature));
    //observed ratios
    double ratioRG = MaxR/MaxG;
    double ratioGB = MaxG/MaxB;
    double ratioRB = MaxR/MaxB;
    
    double ratioDifferences = Math.sqrt(( Math.pow((ratioRG-thRG),2)+Math.pow((ratioGB-thGB),2)+Math.pow((ratioRB-thRB),2))/3);
    
    return ratioDifferences;
  }                                             
  
  // Finds lowest error and returns the temperature at that point ----------------------------
  public static int FindTemperature(double R, double G, double B){
    int temp = 0;
    //just looping through. Idc enough to make this more elegant
    for(int i = 100; i < 10000; i++){
      if(DifferenceCalculation(i+1,R,G,B) < DifferenceCalculation(i,R,G,B)){
        temp = i+1;
      }
    }    
    return temp;
  }
  
}

```
