# sarah7382.github.io
<한달에 3mm씩 자라는 손톱>

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.Random;

public class com {
    // fx = 3x + 1 예상

    public static int MSE(int a, int b, int[] x, int[] y){
        int MSE = 0;
        for(int i = 0 ; i < y.length ; i ++){
            MSE += Math.pow(y[i] - (a*x[i] + b), 2);
        }
        return MSE;
    }

    public static int[] init_A() {
        Random r = new Random();
        int[] arr = new int[4];
        for(int i=0; i<4; i++) {
            arr[i] = r.nextInt(10);
        }
        return arr;
    } //a뽑기

    public static int[] init_B() {
        Random r = new Random();
        int[] arr = new int[4];
        for(int i=0; i<4; i++) {
            arr[i] = r.nextInt(5);
        }
        return arr;
    }  //b뽑기

    public static int[] selection( int[] a, int[] b, int[] x, int[] y) {
        int sum = 0;
        int[] f = new int[a.length];

        for(int i=0; i<a.length; i++) {
            f[i] = MSE(a[i], b[i], x, y);
            sum += f[i];
        }

        for (int i = 0 ; i < f.length ; i++) {
            f[i] = sum - f[i];
        }

        sum = 0;

        for (int i = 0 ; i < f.length ; i++) {
            sum += f[i];
        }

        double[] ratio = new double[a.length];
        for(int i=0; i<a.length; i++) {
            if(i==0) ratio[i] = (double)f[i] / (double)sum;
            else ratio[i] = ratio[i-1] + (double)f[i] / (double)sum;
        }

        int[] sx = new int[a.length];
        Random r = new Random();
        for(int i=0; i<a.length; i++) {
            double p = r.nextDouble();
            if(p < ratio[0]) sx[i] = a[0];
            else if(p < ratio[1]) sx[i] = a[1];
            else if(p < ratio[2]) sx[i] = a[2];
            else sx[i] = a[3];
        }
        return sx;
    }

    public static String int2String(String x) {
        return String.format("%8s", x).replace(' ', '0');
    }

    public static String[] crossOver(int[] x) {
        String[] arr = new String[x.length];
        for(int i=0; i<x.length; i+=2) {
            String bit1 = int2String(Integer.toBinaryString(x[i]));
            String bit2 = int2String(Integer.toBinaryString(x[i+1]));

            arr[i] = bit1.substring(0, 2) + bit2.substring(2, 4);
            arr[i+1] = bit2.substring(0, 2) + bit1.substring(2, 4);
        }
        return arr;
    }

    public static int invert(String x) {
        Random r = new Random();
        int a = Integer.parseInt(x, 2);
        for(int i=0; i<x.length(); i++) {
            double p = (double)1/ (double)30;
            if(r.nextDouble() < p) {
                a = 1 << i ^ a;
            }
        }
        return a;
    }

    public static int[] mutation(String[] x) {
        int[] arr = new int[x.length];
        for (int i=0; i<x.length; i++) {
            arr[i] = invert(x[i]);
        }
        return arr;
    }

    public static void main(String[] args) {
        int[] a = init_A();
        int[] b = init_B();

        int[] x = new int[12];
        for(int i = 0; i < 12; i++) {
            x[i] = i+1;
        }

        int[] y = new int[12];
        for(int i = 0; i < 12; i++) {
            Random r = new Random();
            y[i] = 3* x[i] + 1 + (int)(r.nextInt(4));
            System.out.println(y[i]);
        }

        int result_a = 0;
        int result_b = 0;

        for(int i=0; i<1000; i++) {
            int[] sx = selection(a, b, x, y);
            String[] cx = crossOver(sx);
            int[] mx = mutation(cx);

            int[] f = new int[a.length];
            int min = 9999;
            for(int j = 0; j <a.length; j++) {
                f[j] = MSE(a[j], b[j], x, y);
                if (min > f[j]) {
                    min = f[j];
                    result_a = a[j];
                    result_b = b[j];
                }
                System.out.printf("%d ", f[j]);
            }
            a = mx;
            System.out.println();
            System.out.println(min);
        }
        System.out.println("y = " + result_a + "x + " + result_b);
    }
}


![](https://user-images.githubusercontent.com/62889374/85855787-c6e32e00-b7f1-11ea-8b19-e24ac2adeefa.PNG)
