import java.util.*;

public class Solution {

    static int[][] position;

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        int N =sc.nextInt(); // 점의 개수

        position = new int[N][2]; // 각 점의 위치

        for(int i=0; i<N; i++){
            position[i][0]=sc.nextInt(); // x값 위치 입력
            position[i][1]=sc.nextInt(); // y값 위치 입력
        }

        // x 중앙값 구하기
        int[] xArray = new int [N];
        for(int i=0; i<N; i++){
            xArray[i]=position[i][0];
        }
        double xMed = Xmedian(xArray);

        // 평면 분할
        int firstNod=-1;
        int secondNod=-1;
        ArrayList<Integer> Divide1 = new ArrayList<Integer>(); // 왼쪽 평면
        ArrayList<Integer> Divide2 = new ArrayList<Integer>(); // 오른쪽 평면

        for(int i=0; i<N; i++){
            if(position[i][0]<=xMed){
                Divide1.add(i);
            }
            else{
                Divide2.add(i);
            }
        }

        // 왼쪽 분할에서 최소 값 선정
        double min1=1000;
        int node11 = -1;
        int node12 = -1;
        for(int i=0; i<Divide1.size()-1;i++){
            for(int j=i+1; j<Divide1.size();j++){
                int a = Divide1.get(i);
                int b = Divide1.get(j);
                double tmp = calDist(a,b);
                if(tmp<min1) min1=tmp; node11=a; node12=b;
            }
        }

        // 오른쪽 분할에서 최소 값 선정
        double min2=1000;
        int node21 = -1;
        int node22 = -1;
        for(int i=0; i<Divide2.size()-1;i++){
            for(int j=i+1; j<Divide2.size();j++){
                int a = Divide2.get(i);
                int b = Divide2.get(j);
                double tmp = calDist(a,b);
                if(tmp<min2) min1=tmp; node21=a; node22=b;
            }
        }

        // 두 분할 중 더 최소값을 선정
        double falsemin=0;
        if(min1<=min2) {
            falsemin=min1; firstNod=node11; secondNod=node12;
        }
        else {
            falsemin=min2; firstNod=node21; secondNod=node22;
        }

        int medianXmin = (int) (xMed-falsemin)-1; // x의 중앙값
        int medianXmax = (int) (xMed+falsemin)+1;
        ArrayList<Integer> DivideMed = new ArrayList<Integer>();
        for(int i=0;i<N;i++){
            if((position[i][0]>medianXmin)&&(position[i][0]<medianXmax)){
                DivideMed.add(i);
            }
        }

        // 두 평면 사이 부분의 최소값 산정
        double realmin=falsemin;
        for(int i=0; i<DivideMed.size()-1;i++){
            for(int j=i+1; j<DivideMed.size();j++){
                int a = DivideMed.get(i);
                int b = DivideMed.get(j);
                double tmp = calDist(a,b);
                if(tmp<realmin) {
                    realmin=tmp; firstNod=a; secondNod=b;
                }
            }
        }

        System.out.println(realmin+" "+firstNod+" "+secondNod);

    }

    // 두점 사이의 거리 구하기
    public static double calDist(int a, int b){

        return Math.sqrt(Math.pow(position[a][0]-position[b][0],2)+Math.pow(position[a][1]-position[b][1],2));

    }

}