 ---
 title: 2026-01-20
 author: 강병호 (이름)
 date: 2026-01-20 (날짜)
 category: TIL/강병호/2026/01 (파일 경로 : TIL/{이름}/{연}/{월})
 layout: post (자유)
 ---

```
package Q2417;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Q2961 {

    static int res = Integer.MAX_VALUE;
    public static void solve(int index, int bSum, int N, int sSum, int[] sList, int[] bList, int cnt) {
        // pruning : 최종 도달
        if (index == N && cnt >= 1) {
//            System.out.println("bSum : " + bSum);
//            System.out.println("sSum : " + sSum);
//            System.out.println("Math.abs(bSum - sSum) : " + Math.abs(bSum - sSum));

            res = Math.min(res, Math.abs(bSum - sSum));
        } else if (index == N && cnt == 0) {
            return;
        } else {
            solve(index+1, bSum + bList[index], N, sSum * sList[index], sList, bList, cnt+1);
            solve(index+1, bSum, N, sSum, sList, bList, cnt);
        }
    }

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        // input
        // 재료 N 1~10
        // N개 줄 -> 신맛(S), 쓴맛(B)  총 계산 수 : 1_000_000_000 보다 작은 수

        int N = Integer.parseInt(st.nextToken());
        int[] sList = new int[N];
        int[] bList = new int[N];

        for (int i = 0; i < N; i++) {
            st = new StringTokenizer(br.readLine());
            int S = Integer.parseInt(st.nextToken());
            int B = Integer.parseInt(st.nextToken());

            sList[i] = S;
            bList[i] = B;
        }

        // solve
        // backtracking
        solve(0, 0, N, 1, sList, bList, 0);

        System.out.println(res);

    }
}

```
