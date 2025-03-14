# PTA乙代码

## 1~25

### 1001.**害死人不偿命的(3n+1)猜想**

```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        int n = in.nextInt();
        int count = 0;
        while(n != 1){
            if(n % 2 == 0)    n = n/2;
            else n = (3*n+1)/2;
            count++;
        }
        System.out.println(count);
    }
}
```



### 1002.**写出这个数**

```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        String input = in.next();
        int sum = 0;
		//字符串遍历不好做操作，因此转化为字符型数组
        char[] arr1 = input.toCharArray();
        for(int i = 0;i < arr1.length;i++){
            sum += Character.getNumericValue(arr1[i]);
            //sum+=arr1[i]-48;	//	字符数-48可以把它转化为整型
        }

        String[] arr2 = {"ling","yi","er","san","si","wu","liu","qi","ba","jiu"};
        String sumstr = Integer.toString(sum);
        //遍历结果分别进行对应,注意最后一个无空格，要进行区分
        for(int j = 0;j < sumstr.length();j++){
            int temp = Character.getNumericValue(sumstr.charAt(j));
            //int temp = sumstr.charAt(j) - 48;
            if(j != sumstr.length() - 1)    System.out.print(arr2[temp] + " ");
            else System.out.print(arr2[temp]);
        }
    }
}
```



### 1003.**我要通过！**

```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        int n = in.nextInt();
        in.nextLine();

        for(int i = 0;i < n;i++){
            String s = in.nextLine();
            if(checkString(s))
                System.out.println("YES");
            else    System.out.println("NO");
        }
    }
	//	检查字符串合法性
    private static boolean checkString(String s){
        //	判断字符串是否只含PAT
        String strTemp = s.replace("P","").replace("A","").replace("T","");
        if(!strTemp.isEmpty())    return false;
        //	获取P和T下标
        int pIndex = s.indexOf('P');
        int tIndex = s.indexOf('T');
		//	P下标在T前面
        if (pIndex == -1 || tIndex == -1 || pIndex >= tIndex) {
            return false;
        }
		
        //	获取子串
        String first = s.substring(0, pIndex);
        String second = s.substring(pIndex + 1, tIndex);
        String third = s.substring(tIndex + 1);
		
        //	中间子串至少含有一个A
        if(second.length() == 0)    return false;
        //	T后A个数应当是P前A个数 * 中间A个数
        if(third.length() != first.length() * second.length())    return false;
            
        if(!containsOnlyA(first) || !containsOnlyA(second) || !containsOnlyA(third))
            return false;

            return true;
    }
	//	判断子串是否为全为A或者为空串
    private static boolean containsOnlyA(String s){
        if(s.isEmpty())    return true;
        for(int i = 0;i < s.length();i++){
                if(s.charAt(i) != 'A')    return false;
            }
        return true;
    }
}

//本题必须满足的条件如下：
        // 1.字符串中必须仅有 P、 A、 T这三种字符，不可以包含其它字符；
        // 2.P和T之间不能没有A(A的个数大于等于1，等于0就是错的)
        // 3.开头(P之前)的A的个数 * 中间（P和T中间）的A的个数 = 结尾（T之后）的A的个数，
        // 4.P和T只能有一个
```



### 1004.**成绩排名**

```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        int n = in.nextInt();
        in.nextLine();

        Student[] students = new Student[n];
        for(int i = 0;i < n;i++){
            String str = in.nextLine();
            String[] tempStr = str.split(" ");
            students[i] = new Student(tempStr[0],tempStr[1],Integer.parseInt(tempStr[2]));
        }

        Student maxStudent = students[0],minStudent = students[0];
        for(int j = 0;j < n;j++){
            if(students[j].score > maxStudent.score)    maxStudent = students[j];
            if(students[j].score < minStudent.score)    minStudent = students[j];
        }

        System.out.println(maxStudent.name + " " + maxStudent.number);
        System.out.println(minStudent.name + " " + minStudent.number);
    }

    static class Student{
        String name;
        String number;
        int score;

        Student(String name,String number,int score){
            this.name = name;
            this.number = number;
            this.score = score;
        }
    }
}
```



### 1005.**继续(3n+1)猜想**

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();  // 读取数组的大小
        int[] arr = new int[n];

        // 读取数组中的元素
        for (int i = 0; i < n; i++) {
            arr[i] = scanner.nextInt();
        }

        // 用来存储变换过程中遇到的所有数字
        HashSet<Integer> visited = new HashSet<>();

        // 对每个数字进行变换
        for (int i = 0; i < n; i++) {
            int k = arr[i];
            while (k != 1) {
                if (k % 2 == 1) {  // k 是奇数
                    k = (3 * k + 1) / 2;
                } else {  // k 是偶数
                    k = k / 2;
                }
                visited.add(k);  // 将变换后的数字添加到 visited 集合中
            }
        }

        // 用来存储那些不在 visited 集合中的数字
        List<Integer> resultList = new ArrayList<>();

        // 收集那些在变换过程中没有出现过的数字
        for (int i = 0; i < n; i++) {
            if (!visited.contains(arr[i])) {
                resultList.add(arr[i]);
            }
        }

        // 降序排序
        Collections.sort(resultList, Collections.reverseOrder());

        // 输出结果
        for (int i = 0; i < resultList.size(); i++) {
            if (i > 0) {
                System.out.print(" ");
            }
            System.out.print(resultList.get(i));
        }
    }
}

```



### 1006.**换个格式输出整数**

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        int input = in.nextInt();
        StringBuilder sb = new StringBuilder();

        int Bn = input/100;
        int Sn = (input % 100) / 10;
        int n = input % 10;

        for(int i = 0;i < Bn;i++){
            sb.append("B");
        }
        for(int j = 0;j < Sn;j++){
            sb.append("S");
        }
        for(int k = 0;k < n;k++){
            sb.append(k+1);
        }
        System.out.print(sb);
    }
}
```



### 1007.**素数对猜想**

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        int n = in.nextInt();
        int ans=0;
        for (int i = 5; i <=n ; i++) {
            //从5开始因为前面几个数都不符合，可以直接pass
            if(isPrime(i) && isPrime(i-2))    ans++;
        }
        System.out.println(ans);
    }
    static boolean isPrime(int n) {
        if (n <= 1) return false;              // 1 或更小的数字不是素数
        if (n == 2 || n == 3) return true;     // 2 和 3 是素数
        if (n % 2 == 0 || n % 3 == 0) return false;  // 排除 2 和 3 的倍数
        for (int i = 5; i * i <= n; i += 6) {  // 从 5 开始，每次跳过 6，检查 6k ± 1
            if (n % i == 0 || n % (i + 2) == 0) return false; // 如果 n 能被 i 或 (i + 2) 整除，则不是素数
        }
        return true;  // 如果没有发现能整除的数，返回 true，表示 n 是素数
    }
}
```

**素数和6的关系：**

- 所有的整数可以用以下形式表示：`6k + 0`、`6k + 1`、`6k + 2`、`6k + 3`、`6k + 4`、`6k + 5`。
- 如果一个数的形式是 `6k + 0`、`6k + 2`、`6k + 4`，那么它一定是 2 的倍数，显然不是素数。
- 如果一个数的形式是 `6k + 3`，那么它一定是 3 的倍数，显然也不是素数。
- 唯有 `6k + 1` 和 `6k + 5`（即 `6k - 1`）的数，才有可能是素数。
- 因此该题只需检验小于N的数内所有`6k+1（i+2）` 和 `6k-1（i）` 的值是否为素数（能不能整除其他素数如25可以整除5）

**素数的因子都在 `sqrt(n)`（即 `n` 的平方根）以内：**

如果 `n` 可以被某个数整除，那么它的因子一定是成对出现的，

也就是说，如果 `n = a * b`，那么要么 `a <= sqrt(n)`，要么 `b <= sqrt(n)`。

所以，只需要检查到平方根 `sqrt(n)` 即可，如果没有找到因子，就可以确定 `n` 是素数。



### 1008.**数组元素循环右移问题** 

```java
import java.util.*;

public class Main {
    
    // 反转数组的部分内容
    public static void reverse(int[] arr, int start, int end) {
        while (start < end) {
            int temp = arr[start];
            arr[start] = arr[end];
            arr[end] = temp;
            start++;
            end--;
        }
    }

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int N = in.nextInt();  // 数组大小
        int M = in.nextInt();  // 旋转次数
        int[] arr = new int[N];
        
        // 输入数组元素
        for (int i = 0; i < N; i++) {
            arr[i] = in.nextInt();
        }

        M = M % N;  // 防止 M 大于 N，M > N 的情况下取余

        // 1. 反转整个数组
        reverse(arr, 0, N - 1);
        
        // 2. 反转前 M 个元素
        reverse(arr, 0, M - 1);
        
        // 3. 反转后 N-M 个元素
        reverse(arr, M, N - 1);
        
        // 输出结果
        for (int i = 0; i < N; i++) {
            if (i != N - 1) {
                System.out.print(arr[i] + " ");
            } else {
                System.out.print(arr[i]);
            }
        }
    }
}

```

**M = M % N;  // 防止 M 大于 N，M > N 的情况下取余**

在不使用额外数组的情况下进行循环右移操作，且尽量减少数据移动次数，可以通过三次反转操作来实现整个数组的循环右移：

- 首先，反转整个数组。

- 然后，反转前 `M` 个元素。

- 最后，反转剩下的 `N-M` 个元素。

  

另解（只需要输出而不用操作数组情况下）：

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int N = scanner.nextInt();  // 数组的大小
        int M = scanner.nextInt();  // 旋转的步数
        int[] nums = new int[N];    // 数组存储元素
        
        // 输入数组元素
        for (int i = 0; i < N; i++) {
            nums[i] = scanner.nextInt();
        }
        
        // 计算实际旋转步数，避免旋转次数大于数组长度
        M = M % N;  // 如果 M 大于 N，取余以避免不必要的多次旋转
        
        // 输出旋转后的数组
        for (int i = 0; i < N; i++) {
            // 计算旋转后的索引
            int j = (N - M + i) % N;  // 通过公式计算新的索引
            if (i != N - 1) {
                System.out.print(nums[j] + " ");
            } else {
                System.out.print(nums[j]);
            }
        }
    }
}
```

对于每个索引 `i`，新位置应该是 `(N - M + i) % N`。这个公式的含义是：

- `N - M` 是旋转后数组的起始位置；
- `i` 是当前元素的偏移量；
- `% N` 保证了当索引超出数组边界时会重新回到起始位置。



### 1009.说反话

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        String s = in.nextLine();
        String[] str = s.split(" ");
		
        //	翻转数组
        int start = 0;
        int end = str.length - 1;
        while(start < end){
            String temp = str[start];
            str[start] = str[end];
            str[end] = temp;
            start++;
            end--;
        }

        for(int i = 0;i < str.length;i++){
            if(i != str.length - 1)    System.out.print(str[i] + " ");
            else    System.out.print(str[i]);
        }
    }
}
```



### 1010.**一元多项式求导**

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        List<Integer> coefficients = new ArrayList<>();
        List<Integer> exponents = new ArrayList<>();

        // 读取输入，存入系数和指数列表
        while (in.hasNextInt()) {
            coefficients.add(in.nextInt());
            exponents.add(in.nextInt());
        }

        // 存储导数的系数和指数
        List<Integer> derivationCoefficients = new ArrayList<>();
        List<Integer> derivationExponents = new ArrayList<>();

        // 计算导数
        for (int i = 0; i < coefficients.size(); i++) {
            int coef = coefficients.get(i);
            int exp = exponents.get(i);
            if (exp > 0) {
                // 导数系数为 coef * exp, 导数指数为 exp - 1
                derivationCoefficients.add(coef * exp);
                derivationExponents.add(exp - 1);
            }
        }

        // 输出导数
        if (derivationCoefficients.isEmpty()) {
            System.out.println("0 0");
        } else {
            for (int i = 0; i < derivationCoefficients.size(); i++) {
                System.out.print(derivationCoefficients.get(i) + " " + derivationExponents.get(i));
                if (i != derivationCoefficients.size() - 1) {
                    System.out.print(" ");
                }
            }
        }
        in.close();
    }
}

```

另解（只管输出）：

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        //scanner.hasNext() 回车时候自动退出循环
        int flag=0;
        while (scanner.hasNext()) {
            int a = scanner.nextInt();
            int b = scanner.nextInt();
            if (a != 0 && b != 0) {
                //这个要写在if语句里面，写在外面会多输出一个“ ” 
                if (flag == 1) System.out.print(" ");
                System.out.print((a * b) + " " + (b - 1));
                flag = 1;
            }
        }
        if(flag==0) System.out.print("0 0");
    }
}
```



### 1011.**A+B 和 C**

```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        int n = in.nextInt();
        in.nextLine();
        boolean[] ans = new boolean[n];
        String[] s = new String[n];
        int index = 0;
        
        for(int k = 0;k < n;k++){
            s[k] = in.nextLine();
        }
        for(int i = 0;i < n;i++){
            String[] str = s[i].split(" ");
            if(Long.parseLong(str[0]) + Long.parseLong(str[1]) > Long.parseLong(str[2]))    ans[i] = true;
            else ans[i] = false;
        }

        for(int j = 0;j < n;j++){
            System.out.println("Case #" + (j+1) + ": " + ans[j]);
        }
    }
}
```

**注意太大的数字要用Long**



### 1012.**数字分类**

```java
import java.util.Scanner;
import java.text.DecimalFormat;

public class Main{
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        int n = in.nextInt();
        int[] input = new int[n];
        for(int i = 0;i < n;i++){
            input[i] = in.nextInt();
        }
        String[] ans = new String[5];

        //    A1
        int sum1 = 0;
        int count1 = 0;
        //    A2
        int sum2 = 0;
        int seq = 1;
        int count2 = 0;
        //    A3
        int count3 = 0;
        //    A4
        float sum4 = 0;
        int count4 = 0;
        float avg = 0;
        DecimalFormat df = new DecimalFormat("0.0");
        //    A5
        int count5 = 0;
        int max = input[0];
        for(int i = 0;i < n;i++){
            //    A1
            if(input[i] % 5 == 0){
                if(input[i] % 2 == 0){
                    sum1 += input[i];
                    count1++;
                }
            }
            //    A2
            if(input[i] % 5 == 1){
                if(seq % 2 == 1)    sum2 += input[i];
                else    sum2 -= input[i];
                seq++;
                count2++;
            }
            //    A3
            if(input[i] % 5 == 2){
                count3++;
            }
            //    A4
            if(input[i] % 5 == 3){
                sum4 += input[i];
                count4++;
            }
            //    A5
            if(input[i] % 5 == 4){
                count5++;
                if(max < input[i])    max = input[i];
            }
        }
        if(count1 == 0)    ans[0] = "N";
        else    ans[0] = Integer.toString(sum1);
        if(count2 == 0)    ans[1] = "N";
        else    ans[1] = Integer.toString(sum2);
        if(count3 == 0)    ans[2] = "N";
        else    ans[2] = Integer.toString(count3);
        if(count4 == 0)    ans[3] = "N";
        else{
            avg = sum4 / count4;
            ans[3] = df.format(avg);
        }
        if(count5 == 0)    ans[4] = "N";
        else    ans[4] = Integer.toString(max);
        for(int i = 0;i < 5;i++){
            if(i != 4)    System.out.print(ans[i] + " ");
            else    System.out.print(ans[i]);
        }
    }
}
```



### 1013.**数素数**

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        int start = in.nextInt();
        int end = in.nextInt();

        int count = 0;	// 素数个数
        int seq = 0;	// 数到第几个素数
        int i = 2;		// 第一个素数是2
        List<Integer> ans = new ArrayList<>();
        while(seq < end){
            if(isPrime(i)){
                seq++;
                if(seq >= start){
                    ans.add(i);
                    count++;
                }
            }
            i++;
        }

        int num = 0;
        for(int j = 0;j < count;j++){
            num++;
            if(num % 10 != 1)    System.out.print(" ");
            System.out.print(ans.get(j));
            if(num % 10 == 0)    System.out.println();
        }
    }
    static boolean isPrime(int n){
        if(n <= 1)    return false;
        if(n == 2 || n == 3)    return true;
        if(n % 2 == 0 || n % 3 == 0)    return false;
        for(int i = 5;i * i <= n;i += 6){
            if(n % i == 0 || n % (i+2) == 0)    return false;
        }
        return true;
    }
}
```

**注意这里的打印10个数字换行的处理方法**



### 1014.**福尔摩斯的约会**

```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        char[] ch1 = in.nextLine().toCharArray();
        char[] ch2 = in.nextLine().toCharArray();
        char[] ch3 = in.nextLine().toCharArray();
        char[] ch4 = in.nextLine().toCharArray();
        String[] day = {"MON","TUE","WED","THU","FRI","SAT","SUN"};
        char[] ans = new char[2];

        int flag = 0;
        for(int i = 0;i < Math.min(ch1.length,ch2.length);i++){
            if(flag == 0 && ch1[i] == ch2[i] && ch1[i] >= 'A' && ch1[i] <= 'G'){
                ans[0] = ch1[i];
                flag++;
            }
            else if(flag == 1 && ch1[i] == ch2[i]){
                if(ch1[i] >= '0' && ch1[i] <= '9' || ch1[i] >= 'A' && ch1[i] <= 'N'){
                    ans[1] = ch1[i];
                    break;
                }
            }
        }

        int minute = 0;
        for(int j = 0;j < Math.min(ch3.length,ch4.length);j++){
            if(ch3[j] >= 'A' && ch3[j] <= 'Z' || ch3[j] >= 'a' && ch3[j] <= 'z'){
                if(ch3[j] == ch4[j])    minute = j;
            }
        }

        System.out.print(day[ans[0] - 'A'] + " ");
        if(Character.isDigit(ans[1])){
            System.out.printf("%02d:",ans[1] - '0');
        }else System.out.printf("%02d:",ans[1] - 'A' + 10);
        System.out.printf("%02d",minute);
    }
}
// 判断条件：
// 第一对相同字符必须是大写字母，且在A~G（一周七天）
// 第二对相同字符有两种情况：0~9 和 A~N（表示0点~23点）
// 第三队相同字符必须是英文字母，既可以是大写也可以是小写
```

1. **注意System.out.printf("%02d",minute);这里是 `printf`**

   + %d ：正常输出十进制数 。
   + %Yd：十进制数，输出 Y 位。如果本身大于 Y 位，正常输出。
   + %XYd：十进制数，输出 Y 位，不足 Y 位就补 X 。如果本身大于 Y 位，正常输出。

   以 %d，%2d， %02d 为例，

   - %d：十进制数正常输出 。
   - %2d：十进制数，输出 2 位。不足 2 位就补空格 。如果本身大于 2 位，正常输出。
   - %02d ：十进制数，输出 2 位，不足 2 位就补 0 。如果本身大于 2 位，正常输出。

2. `Character.isDigit(char ch)` 用于判断字符是否为数字



### 1015.**德才论**

```java
import java.io.*;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
 
// 此类考生按德才总分从高到低排序  （降序）
// 当某类考生中有多人总分相同时，按其德分降序排列；
// 若德分也并列，则按准考证号的升序输出
public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    
    public static void main(String[] args) throws IOException {
       in.nextToken();
        int N = (int) in.nval;    // 考生总数
        in.nextToken();
        int L = (int) in.nval;    // 录取最低分数线
        in.nextToken();
        int H = (int) in.nval;    // 优先录取线
        int M = 0;
        // 四类考生
        ArrayList<Student> list1 = new ArrayList<>();
        ArrayList<Student> list2 = new ArrayList<>();
        ArrayList<Student> list3 = new ArrayList<>();
        ArrayList<Student> list4 = new ArrayList<>();

        for(int i = 0;i < N;i++){
            in.nextToken();
            int CandidateNumber = (int) in.nval;
            in.nextToken();
            int MoralScore = (int) in.nval;
            in.nextToken();
            int TalentScore = (int) in.nval;
            if(MoralScore >= L && TalentScore >= L){
                M++;
                if(MoralScore >= H && TalentScore >= H){    // 第一类考生:“才德全尽”
                    list1.add(new Student(CandidateNumber,MoralScore,TalentScore));
                }else if(MoralScore >= H && TalentScore < H){    // 第二类考生:“德胜才”
                    list2.add(new Student(CandidateNumber,MoralScore,TalentScore));
                }else if(MoralScore < H && TalentScore < H && MoralScore >= TalentScore){  // 第三类考生:“才德兼亡”但尚有“德胜才”
                    list3.add(new Student(CandidateNumber,MoralScore,TalentScore));
                }else    list4.add(new Student(CandidateNumber,MoralScore,TalentScore));    // 其余达标考生
            }
        }
        // 排序
        Collections.sort(list1);
        Collections.sort(list2);
        Collections.sort(list3);
        // 对于只有一个列表排序时也可以直接重写Comparator而不用在Student类中实现Comparable接口
        Collections.sort(list4,new Comparator<Student>(){
            public int compare(Student o1,Student o2){
                // 先按总分降序排序
                if(o1.sum != o2.sum)    return o2.sum - o1.sum;
                // 总分相同按德分降序
                else if(o1.MoralScore != o2.MoralScore)    return o2.MoralScore - o1.MoralScore;
                // 若德分也并列，则按准考证号的升序输出
                else return o1.CandidateNumber - o2.CandidateNumber;
            }
        });
        // 输出
        System.out.println(M);
        for(int i = 0;i < list1.size();i++){
            out.println(list1.get(i).CandidateNumber + " " + list1.get(i).MoralScore + " " + list1.get(i).TalentScore);
        }
        for(int i = 0;i < list2.size();i++){
            out.println(list2.get(i).CandidateNumber + " " + list2.get(i).MoralScore + " " + list2.get(i).TalentScore);
         }
        for(int i = 0;i < list3.size();i++){
            out.println(list3.get(i).CandidateNumber + " " + list3.get(i).MoralScore + " " + list3.get(i).TalentScore);
        }
        for(int i = 0;i < list4.size();i++){
            out.println(list4.get(i).CandidateNumber + " " + list4.get(i).MoralScore + " " + list4.get(i).TalentScore);
        }
        out.flush();
    }
    
    static class Student implements Comparable<Student>{
        int CandidateNumber;
        int MoralScore;
        int TalentScore;
        int sum;

        Student(int CandidateNumber,int MoralScore,int TalentScore){
            this.CandidateNumber = CandidateNumber;
            this.MoralScore = MoralScore;
            this.TalentScore = TalentScore;
            this.sum = MoralScore + TalentScore;
        }

        public int compareTo(Student o){
            // 先按总分降序排序
            if(this.sum != o.sum)    return o.sum - this.sum;
            // 总分相同按德分降序
            else if(this.MoralScore != o.MoralScore)    return o.MoralScore - this.MoralScore;
            // 若德分也并列，则按准考证号的升序输出
            else return this.CandidateNumber - o.CandidateNumber;
        }
    }
}
// 测试点3,4超时
```



### 1016.**部分A+B**

```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        char[] A = in.next().toCharArray();
        char DA = in.next().charAt(0);
        char[] B = in.next().toCharArray();
        char DB = in.next().charAt(0);

        StringBuilder PA = new StringBuilder();
        StringBuilder PB = new StringBuilder();
        for(int i = 0;i < A.length;i++){
            if(A[i] == DA)    PA.append(A[i]);
        }
        for(int i = 0;i < B.length;i++){
            if(B[i] == DB)    PB.append(B[i]);
        }
        if(PA.toString() == "")    PA.append(0);
        if(PB.toString() == "")    PB.append(0);
        System.out.print(Integer.parseInt(PA.toString()) + Integer.parseInt(PB.toString()));
    }
}
```





### 1017 .**A除以B**

```java
import java.util.Scanner;
import java.math.BigInteger;
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        String[] big = ins.readLine().split(" ");
        BigInteger b1 = new BigInteger(big[0]);
        BigInteger b2 = new BigInteger(big[1]);
        out.print(b1.divide(b2) + " ");
        out.print(b1.mod(b2));
        out.flush();
    }
}
```



### 1018.**锤子剪刀布**

```java
import java.io.*;
import java.util.HashMap;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int rounds = (int) in.nval;    // 因为in.nval默认输入的是double类型
        char[] arr1 = new char[rounds];
        char[] arr2 = new char[rounds];
        String[] s = new String[2];
        for(int i = 0;i < rounds;i++){
            s = ins.readLine().split(" ");
            arr1[i] = s[0].charAt(0);
            arr2[i] = s[1].charAt(0);
        }
		
        // 胜利手势和索引映射
        HashMap<Character,Integer> map = new HashMap<Character,Integer>();
        map.put('B',0);
        map.put('C',1);
        map.put('J',2);
        HashMap<Integer,Character> ges = new HashMap<Integer,Character>();
        ges.put(0,'B');
        ges.put(1,'C');
        ges.put(2,'J');
        
        int avictory = 0;	// 甲胜利次数
        int bvictory = 0;
        int alose = 0;		// 甲失败次数
        int blose = 0;
        int draw = 0;		// 平局数
        int[] avicges = new int[3];	// 甲各手势胜利次数，索引映射与map相同
        int[] bvicges = new int[3];
        
        for(int i = 0;i < rounds;i++){
            char a = arr1[i];
            char b = arr2[i];
            if(a == b){
                draw++;
                continue;
            }
            if(beat(a,b)){
                avictory++;
                blose++;
                avicges[map.get(a)]++;
            }else{
                alose++;
                bvictory++;
                bvicges[map.get(b)]++;
            }
        }
        
        int amvicges = avicges[0];    // 胜利手势最大值
        int bmvicges = bvicges[0];
        int ami = 0;                  // 胜利手势最大值索引
        int bmi = 0;
        for(int i = 0;i < 3;i++){
            if(amvicges < avicges[i]){
                amvicges = avicges[i];
                ami = i;
            }
            if(bmvicges < bvicges[i]){
                bmvicges = bvicges[i];
                bmi = i;
            }
        }
        char agesmost = ges.get(ami);
        char bgesmost = ges.get(bmi);
        
        out.println(avictory + " " + draw + " " + alose);
        out.println(bvictory + " " + draw + " " + blose);
        out.println(agesmost + " " + bgesmost);
        out.flush();
    }
    // 判读甲是否胜利
    static boolean beat(char a,char b){
        if(a == 'C' && b == 'J' ||
          a == 'J' && b == 'B' ||
          a == 'B' && b == 'C')    return true;
        else return false;
    }
}
// 测试点5超时
```



### 1019.**数字黑洞**

```java
import java.io.*;
import java.util.Arrays;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int num = (int) in.nval;
        int[] nums = new int[4];
        
        while(true){
            for(int i = 0;i < nums.length;i++){
                nums[i] = num % 10;
                num = num / 10;
            }
            Arrays.sort(nums);
            int a = nums[0] * 1000 + nums[1] * 100 + nums[2] * 10 + nums[3];
            int b = nums[3] * 1000 + nums[2] * 100 + nums[1] * 10 + nums[0];
            int res = b - a;
            if(res == 0){
                out.printf("%04d - %04d = %04d",b,a,res);
                out.flush();
                return;
            }else{
                out.printf("%04d - %04d = %04d\n",b,a,res);    // 记得换行\n
                if(res == 6174){
                    out.flush();
                    return;
                }
            }
            num = res;    // 更新num值
        }
    }
}
```

我的解：

```java
import java.util.Scanner;
import java.util.Arrays;

public class Main{
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        String s = in.next();
        int num = DigitSort(Integer.parseInt(s));
        int rvnum = reverseNumber(num);

        int res = rvnum - num;
        if(res == 0)    System.out.printf("%04d - %04d = %04d",rvnum,num,res);
        else{
            while(res != 6174){
                System.out.printf("%04d - %04d = %04d\n",rvnum,num,res);
                num = DigitSort(res);
                rvnum = reverseNumber(num);
                res = rvnum - num;	
            }
            // 当第一次运算结果就是6174时要确保一定会输出该式子（8532 - 2358 = 6174）
            System.out.printf("%04d - %04d = %04d\n",rvnum,num,res);	
        }
    }
	
    // 获得升序序列
    static int DigitSort(int a){
        String sf = String.format("%04d",a);	// 确保为4位数
        char[] num = sf.toCharArray();
        Arrays.sort(num);
        String s = new String(num);
        return Integer.parseInt(s);
    }
	
    // 获得降序序列
    static int reverseNumber(int a){
        String sf = String.format("%04d",a);	// 确保为4位数
        char[] num = sf.toCharArray();
        Arrays.sort(num);
        reverse(num,0,3);
        String s = new String(num);
        return Integer.parseInt(s);
    }
	
    // 反转数组
    static void reverse(char[] num,int start,int end){
        while(start < end){
            char temp = num[start];
            num[start] = num[end];
            num[end] = temp;
            start++;
            end--;
        }
    }
}
```



### 1020.月饼

```java
import java.io.*;
import java.util.ArrayList;
import java.util.Collections;
import java.text.DecimalFormat;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        in.nextToken();
        int maxDemand = (int) in.nval;
        String[] s = ins.readLine().split(" ");
        String[] tp = ins.readLine().split(" ");
        double[] stock = new double[n];
        double[] TotalPrice = new double[n];
        double price = 0.00;
        ArrayList<MoonCake> list = new ArrayList<>();

        for(int i = 0;i < n;i++){
            stock[i] = Double.parseDouble(s[i]);
            TotalPrice[i] = Double.parseDouble(tp[i]);
            price = TotalPrice[i] / stock[i];
            list.add(new MoonCake(stock[i],TotalPrice[i],price));
        }

        // 单价从高到低排序
        Collections.sort(list);

        // 计算最大收益
        double maxProfit = 0.00;
        int remainDemand = maxDemand;    // 剩余需求量
        for(int i = 0;i < n;i++){
            if(remainDemand == 0)    break;
            if(remainDemand >= list.get(i).stock){    // 剩余需求大于当前种类月饼库存量
                maxProfit += list.get(i).TotalPrice;
                remainDemand -= list.get(i).stock;
            }else{
                maxProfit += remainDemand * list.get(i).price;
                remainDemand = 0;
            }
        }
        DecimalFormat df = new DecimalFormat("0.00");
        out.printf(df.format(maxProfit));
        out.flush();
    }

    static class MoonCake implements Comparable<MoonCake>{
        double stock;
        double TotalPrice;
        double price;

        MoonCake(double stock,double TotalPrice,double price){
            this.stock = stock;
            this.TotalPrice = TotalPrice;
            this.price = price;
        }
        // 注意对于非int类型值重写compareTo函数的写法
        public int compareTo(MoonCake o){
            if(this.price > o.price)   return -1;
            else if(this.price < o.price)   return 1;
            else return 0;
        }
    }
}
```



### 1021.**个位数统计**

```java
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        String s = ins.readLine();
        char[] digit = s.toCharArray();
        int[] frequency = new int[10];	// 各位数字出现频率
		
        // 统计各位数字出现频率
        for(int i = 0;i < digit.length;i++){
            frequency[digit[i] - '0']++;	
        }
        // 输出出现过的数字
        for(int i = 0;i < 10;i++){
            if(frequency[i] != 0){
                out.println(i + ":" + frequency[i]);
            }
        }
        out.flush();
    }
}
```



### 1022.**D进制的A+B**

```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        int a = in.nextInt();
        int b = in.nextInt();
        int d = in.nextInt();
		// 进制转换利用整型转字符串方法的重载方法Integer.toString(int i,int radix)
        System.out.println(Integer.toString(a+b,d));
    }
}
```



### 1023.**组个最小数**

```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        String[] s = in.nextLine().split(" ");
        int[] frequency = new int[10];
        for(int i = 0;i < frequency.length;i++){
            frequency[i] = Integer.parseInt(s[i]);
        }

        StringBuilder sb = new StringBuilder();
        int flag = 0;    // 第一个出现的数字
        for(int i = 1;i < frequency.length;i++){
            if(flag == 0){
                if(frequency[i] != 0){
                    flag = i;
                }
            }else    break;
        }

        sb.append(flag);    // 拼上第一个数字
        frequency[flag]--;  // 后续第一个数字需要拼接的次数-1
        // 拼接上所有0
        for(int i = 0;i < frequency[0];i++){
            sb.append(0);
        }
        
        // 升序拼接剩余出现过的数字
        for(int i = flag;i < frequency.length;i++){
            while(frequency[i] != 0){
                sb.append(i);
                frequency[i]--;
            }
        }
        // 输出
        System.out.print(sb);
    }
}
```



### 1024.**科学计数法**

```java
import java.io.*;
import java.math.BigDecimal;

public class Main {
    static PrintWriter out=new PrintWriter(System.out);
    static BufferedReader ins=new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in=new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException {
        BigDecimal sd=new BigDecimal(ins.readLine());
        System.out.println(sd.toPlainString());
    }
}
```

| 方法              | 描述                                                         | 示例输出                             |
| ----------------- | ------------------------------------------------------------ | ------------------------------------ |
| `toString()`      | 返回 `BigDecimal` 的字符串表示，可能使用科学记数法（视数字大小而定）。 | `1.234567890000000E15`（科学记数法） |
| `toPlainString()` | 返回 `BigDecimal` 的字符串表示，**始终使用普通十进制格式**，不使用科学记数法。 | `1234567890000000`（普通表示）       |

**适用场景：**

- **`toString()`**：当你不关心科学记数法的使用，只想得到一个合理的 `BigDecimal` 字符串表示时，可以使用 `toString()`。如果数值范围较大或较小时，它会自动转换为科学记数法。
- **`toPlainString()`**：当你需要确保输出始终为**普通数字表示**时（例如需要输出精确数字，避免科学记数法），应该使用 `toPlainString()`。



### 1025.**反转链表中每K个结点**

```java
import java.io.*;
import java.util.ArrayList;
 
/**
 * @author yx
 * @date 2022-07-15 13:16
 */
public class Main {
    static PrintWriter out=new PrintWriter(System.out);
    static BufferedReader ins=new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in=new StreamTokenizer(ins);
 
    static class Node{
        int Address;
        int Data;
        int Next;
        Node(int Address,int Data,int Next){
            this.Address=Address;
            this.Data=Data;
            this.Next=Next;
        }
    }
 
    public static void main(String[] args) throws IOException {
        in.nextToken();
        int initAddress=(int) in.nval;
        in.nextToken();
        int N=(int) in.nval;//输入的数字组数
        in.nextToken();
        int K=(int) in.nval;//需要反转的节点个数
        ArrayList<Node> list=new ArrayList<>();//定义一个新链表
        Node[] nums=new Node[100000];    // 结点地址为5位非负整数
        
        for (int i = 0; i < N; i++) {
            in.nextToken();
            int address=(int) in.nval;
            in.nextToken();
            int data=(int) in.nval;
            in.nextToken();
            int next=(int) in.nval;
            nums[address]=new Node(address,data,next);
        }
        //生成一个原始链表
        while (initAddress!=-1){
            list.add(nums[initAddress]);
            initAddress=nums[initAddress].Next;
        }

        // 反转链表
        for(int i = 0;i+k-1 < list.size();i += k){    // 外层控制每k个结点
            int start = i;
            int end = i+k-1;
            while(start < end){
                Node temp = list.get(start);
                list.set(start,list.get(end));
                list.set(end,temp);
                start++;
                end--;
            }
        }
        /*
        这个地方一定要注意不能用n代替list.size()，这个点很关键，因为n代表的是输入
        但实际上是输入进去的数据不一定都是有用的，当无效输入时是不会加入链表的
        （输⼊样例中有不在链表中的结点的情况）
        一开始下面全部用n，会导致最后一个测试点非零返回
        把n改成list.size()，最后一个测试点通过
         */

        // 输出
        for(int i = 0;i < list.size()-1;i++){
            list.get(i).next = list.get(i+1).address;    // 改变反转结点的next指针
            out.printf("%05d %d %05d\n",list.get(i).address,list.get(i).data,list.get(i).next);
        }
        out.printf("%05d %d -1\n",list.get(list.size()-1).address,list.get(list.size()-1).data);
        out.flush();
    }
}
// 测试点5超时
```



## 26~50

### 1026.程序运行时间

```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        int c1 = in.nextInt();
        int c2 = in.nextInt();
        double CLK_TCK = 100.00;

        double time = (c2 - c1) / CLK_TCK;
        int hours = (int) time / 3600;
        int mins = (int) (time / 60) % 60;
        // int seconds = (int) Math.round((time - hours * 3600 - mins * 60));
        // int本身向下取整,可以通过加0.5实现向上取整
        int seconds = (int)(time % 60 + 0.5);
        System.out.printf("%02d:%02d:%02d",hours,mins,seconds);
    }
}
```



### 1027.**打印沙漏**

```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        int n = in.nextInt();
        char symbol = in.next().charAt(0);

        // 所用符号总数应该符合2n^2 - 1的规律
        // 此时第一行的符号数应当是2n-1
        int count = 1;
        while(2*count*count-1 <= n){    // 一定要取等，此时刚好全部用上
            count++;
        }
        count--;
        int remain = n - (2*count*count-1);
        // 输出上半部分
        for(int i = count;i > 0;i--){
            for(int j = 0;j < count-i;j++)    System.out.print(" ");
            for(int j = 0;j < 2*i-1;j++)    System.out.print(symbol);
            System.out.print("\n");
        }
        // 输出下半部分
        for(int i = 2;i <= count;i++){
            for(int j = 0;j < count-i;j++)    System.out.print(" ");
            for(int j = 0;j < 2*i-1;j++)    System.out.print(symbol);
            System.out.print("\n");
        }
        // 输出剩余符号数量
        System.out.println(remain);
    }
}
```



### 1028.**人口普查**

```java
import java.io.*;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);

    static class Person implements Comparable<Person>{
        String name;
        String birth;
        int year;
        int month;
        int day;

        Person(String name,String birth){
            this.name = name;
            this.birth = birth;
            String[] date = birth.split("/");
            this.year = Integer.parseInt(date[0]);
            this.month = Integer.parseInt(date[1]);
            this.day = Integer.parseInt(date[2]);
        }
        @Override
        // 按生日年份降序排序（年龄升序：年份越小年龄越大）
        public int compareTo(Person o){
            if(this.year != o.year)    return o.year - this.year;
            if(this.month != o.month)    return o.month - this.month;
            return o.day - this.day;
         }
   }
    
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        ArrayList<Person> list = new ArrayList<>();
        int validcount = n;
        for(int i = 0;i < n;i++){
            String[] s = ins.readLine().split(" ");
            Person p = new Person(s[0],s[1]);
            if(p.year < 1814 || 
               p.year == 1814 && p.month < 9 || 
               p.year == 1814 && p.month == 9 && p.day < 6){
                validcount--;
                continue;
            }
            if(p.year > 2014 || 
               p.year == 2014 && p.month > 9 || 
               p.year == 2014 && p.month == 9 && p.day > 6){
                    validcount--;
                    continue;
               }
            list.add(p);
        }
        Collections.sort(list);
        
        out.print(validcount);
        // 考虑全部都不符合的情况
        if(validcount != 0)
            out.print(" " + list.get(list.size()-1).name + " " + list.get(0).name);
        out.flush();
    }
}
// 测试点4超时
```



### 1029.**旧键盘**

```java
import java.io.*;
import java.util.LinkedHashSet;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        String CorrectInput = ins.readLine();  // 读取应该输入的文字
        String PracticalInput = ins.readLine(); // 读取实际输入的文字
        LinkedHashSet<Character> set = new LinkedHashSet<>(); // 保证顺序并去重

        int j = 0;  // 用于遍历实际输入的字符
        // 遍历应该输入的文字
        for (int i = 0; i < CorrectInput.length(); i++) {
            // 如果当前字符在实际输入中没有找到
            if (j < PracticalInput.length() && CorrectInput.charAt(i) == PracticalInput.charAt(j)) {
                j++;  // 如果字符匹配，移动实际输入的指针
            } else {
                // 否则，该字符是坏掉的键
                set.add(Character.toUpperCase(CorrectInput.charAt(i)));
            }
        }

        // 输出坏掉的键
        for (char c : set) {
            out.print(c);
        }
        out.flush();
    }
}
```

#### `toUpperCase()` 和`toLowerCase()`**转大小写对非字母不做处理**

不用LinkedHashSet的另解：

```java

import java.io.*;
import java.util.HashMap;
import java.util.Locale;

public class Main {
    static PrintWriter out=new PrintWriter(System.out);
    static BufferedReader ins=new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in=new StreamTokenizer(ins);
 
    public static void main(String[] args) throws IOException {
        char[] s1=ins.readLine().toUpperCase().toCharArray();
        String s2=ins.readLine().toUpperCase();
        String ans="";
        for (int i = 0; i < s1.length; i++) {
            if(!s2.contains(s1[i]+"")&&!ans.contains(s1[i]+"")){	// 结果和实际输入均不包含当前字符
                ans=ans+(s1[i]+"");
            }
        }
        System.out.println(ans);
    }
}
```



### 1030.完美数列 

```java
import java.io.*;
import java.util.Arrays;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);

    public static void main(String[] args) throws IOException {
        in.nextToken();
        int n = (int) in.nval;
        in.nextToken();
        long p = (long) in.nval;
        
        int[] nums = new int[n];
        for (int i = 0; i < n; i++) {
            in.nextToken();
            nums[i] = (int) in.nval;
        }
        
        // 排序数组
        Arrays.sort(nums);
        
        // 双指针方法
        int result = 0;
        int start = 0;
        for (int end = start+result; end < n; end++) {	
            // 如果下一个start加上已知最大长度不满足条件，则后续也不可能满足，直接更新start
            while (nums[end] > p * nums[start]) {	// 如果 nums[end] > p * nums[start]，则移动 start 使条件恢复
                start++;
            }
            // 更新最大长度
            result = Math.max(result, end - start + 1);
        }
        
        out.println(result);
        out.flush();
    }
}
// 测试点4超时
```



### **1031.查验身份证**

```java
import java.io.*;
import java.util.ArrayList;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);

    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        ArrayList<String> list = new ArrayList<>();
        int[] weigh = {7,9,10,5,8,4,2,1,6,3,7,9,10,5,8,4,2};
        char[] m = {'1','0','X','9','8','7','6','5','4','3','2'};
        
        for(int i = 0; i < n;i++){
            String s = ins.readLine();
            int z = 0;
            boolean flag = true;    // 前17为是否全为数字
            for(int j = 0;j < 17;j++){
                if(!Character.isDigit(s.charAt(j))){    // 当前字符不是数字
                    flag = false;
                    break;
                }else    z += weigh[j] * Integer.parseInt(String.valueOf(s.charAt(j)));
            }
            if(!flag)    list.add(s);    // 前17位不全为数字
            else{
                z = z % 11;
                char Mvalue = m[z];
                if(Mvalue != s.charAt(17))    list.add(s);
            }
        }
        
        if(list.size() == 0)    out.print("All passed");
        else{
            for(int i = 0;i < list.size();i++){
                out.println(list.get(i));
            }
        }
        out.flush();
    }
}
```



### 1032.**挖掘机技术哪家强**

```java
import java.io.*;
import java.util.HashMap;
import java.util.Map;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);

    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        HashMap<Integer,Integer> map = new HashMap<>();

        for(int i = 0;i < n;i++){
            in.nextToken();
            int number = (int) in.nval;
            in.nextToken();
            int score = (int) in.nval;
            if(!map.containsKey(number))    map.put(number,score);
            else    map.put(number,map.getOrDefault(number,0) + score);
        }
		
        // 这里一定要设为-1不然有个样例不过
        int maxNumber = -1;
        int maxScore = -1;
        for (Map.Entry<Integer, Integer> entry : map.entrySet()){
            if(maxScore < entry.getValue()){
                maxNumber = entry.getKey();
                maxScore = entry.getValue();
            }
        }
        out.print(maxNumber + " " + maxScore);
        out.flush();
    }
}
// 测试点3超时
```



### 1033.旧键盘打字

```java
import java.io.*;

public class Main {
    static PrintWriter out=new PrintWriter(System.out);
    static BufferedReader ins=new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in=new StreamTokenizer(ins);

    public static void main(String[] args) throws IOException {
        String s1 = ins.readLine();
        String s2 = ins.readLine();
        String sup = s1.toUpperCase();  // 坏键字符字母转大写
        String slow = s1.toLowerCase(); // 坏键字符字母转小写
        StringBuilder ans = new StringBuilder();
        
        for (int i = 0; i < s2.length(); i++) {
            if(s1.contains("+")){   // 上档键坏键
                if(s2.charAt(i)<'A'||s2.charAt(i)>'Z') {    // 当前字符不是大写字符
                    if(!sup.contains(s2.charAt(i)+"") && !slow.contains(s2.charAt(i)+""))	// 如果当前字符大小写不是坏键字符
                        ans.append(s2.charAt(i));
                    continue;
                }
            } else if(!sup.contains(s2.charAt(i)+"") && !slow.contains(s2.charAt(i)+"")){   // 如果当前字符大小写不是坏键字符
                ans.append(s2.charAt(i));
            }
        }
        out.print(ans);
        out.flush();
    }
}
```

HashSet解法：

```java
import java.io.*;
import java.util.HashSet;
import java.util.Set;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));

    public static void main(String[] args) throws IOException {
        String s1 = ins.readLine();  // 坏键字符
        String s2 = ins.readLine();  // 要输入的文本

        // 使用 HashSet 来存储坏掉的字符
        Set<Character> brokenKeys = new HashSet<>();
        
        // 添加坏键字符
        for (char ch : s1.toCharArray()) {
            brokenKeys.add(ch);  // 添加非字母字符到坏键集合
            if (Character.isLetter(ch)) {
                char upperChar = Character.toUpperCase(ch);
                char lowerChar = Character.toLowerCase(ch);
                brokenKeys.add(upperChar);  // 如果字符是字母，加入其大写形式
                brokenKeys.add(lowerChar);  // 如果字符是字母，加入其小写形式
            }
        }

        StringBuilder result = new StringBuilder();
        boolean isShiftBroken = brokenKeys.contains('+');  // 上档键是否坏了

        for (int i = 0; i < s2.length(); i++) {
            char currentChar = s2.charAt(i);

            // 如果上档键坏了且当前字符是大写字母，跳过
            if (isShiftBroken && Character.isUpperCase(currentChar)) {
                continue;
            }

            // 如果当前字符在坏键集合中，跳过
            if (brokenKeys.contains(currentChar)) {
                continue;
            }

            // 将符合要求的字符添加到结果中
            result.append(currentChar);
        }

        // 输出最终结果
        out.print(result.toString());
        out.flush();
    }
}

```



### 1034.**有理数四则运算**

```java
import java.io.*;

public class Main {
    static PrintWriter out=new PrintWriter(System.out);
    static BufferedReader ins=new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in=new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException {
        //分步骤切割字符串
        String[] split=ins.readLine().split(" ");
        String[] fir=split[0].split("/");
        String[] sec=split[1].split("/");
        // 较大值用长整型long（int * int可能超过int）
        long a1=Integer.parseInt(fir[0]);
        long b1=Integer.parseInt(fir[1]);
        long a2=Integer.parseInt(sec[0]);
        long b2=Integer.parseInt(sec[1]);

        if(b1!=0 && b2!=0){
            add(a1,b1,a2,b2);
            minus(a1,b1,a2,b2);
            multiply(a1,b1,a2,b2);
            divide(a1,b1,a2,b2);
        }
        out.flush();
    }
    //处理有理数
    static void simplify(long numerator,long denominator){
        if(numerator == 0){
            out.print(0);
            return;
        }

        boolean flag = false;   // 判断值是否为负
        if(numerator < 0){      // 值为负时输出左括号
            out.print("(-");
            numerator = -numerator;
            flag = true;
        }

        //之后化简分子分母均为正数
        // 找最大公因数
        long g = gcd(numerator,denominator);
        numerator = numerator / g;
        denominator = denominator / g;
        // 输出
        if (numerator % denominator == 0) {             // 为整数
            out.print(numerator / denominator);
        } else if (numerator > denominator) {           // 分子大于分母但不为整数
            out.print(numerator / denominator + " " + (numerator % denominator) + "/" + denominator);
        } else if (numerator == denominator) {          // 分子等于分母
            out.print(1);
        } else {                                        // 分子小于分母
            out.print(numerator + "/" + denominator);
        }

        if(flag){   // 值为负时输出右括号
            out.print(")");
        }
    }
    //加
    static void add(long a1,long b1,long a2,long b2){
        simplify(a1,b1);
        out.print(" + ");
        simplify(a2,b2);
        out.print(" = ");
        simplify((a1 * b2) + (a2 * b1),b1 * b2);
        out.println();
    }
    //减
    static void minus(long a1,long b1,long a2,long b2){
        simplify(a1,b1);
        out.print(" - ");
        simplify(a2,b2);
        out.print(" = ");
        simplify((a1 * b2) - (a2 * b1),b1 * b2);
        out.println();
    }
    //乘
    static void multiply(long a1,long b1,long a2,long b2){
        simplify(a1,b1);
        out.print(" * ");
        simplify(a2,b2);
        out.print(" = ");
        simplify(a1 * a2,b1 * b2);
        out.println();
    }
    //除
    static void divide(long a1,long b1,long a2,long b2){
        simplify(a1,b1);
        out.print(" / ");
        simplify(a2,b2);
        out.print(" = ");
        if(a2==0){
            out.println("Inf");
            return;
        }
        if(a2<0){
            simplify(-1 * (a1 * b2),-1 * (a2 * b1));
        }else {
            simplify(a1 * b2,a2 * b1);
        }
    }

    //最大公约数
    static long gcd(long a,long b){
        while(b != 0){
            long temp = b;
            b = a % b;
            a = temp;
        }
        return a;
    }
}
```



### 1035.**插入与归并** 

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Arrays;

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        String[] pre = br.readLine().split(" ");
        String[] later = br.readLine().split(" ");
        int[] original = new int[n];
        int[] current = new int[n];

        // 将输入的字符串数组转换为整数数组
        for (int i = 0; i < n; i++) {
            original[i] = Integer.parseInt(pre[i]);
            current[i] = Integer.parseInt(later[i]);
        }

        // 识别排序类型：判断是否是插入排序
        int i = 0;
        while (current[i] <= current[i + 1] && i < n - 1) {	// 找到排序好的最后一个位置
            i++;
        }

        boolean isInsert = true;
        for (int j = i + 1; j < n; j++) {	// 验证后续无序序列和初始序列是否相同
            if (original[j] != current[j]) {
                isInsert = false;
                break;
            }
        }

        // 插入排序
        if (isInsert) {
            System.out.println("Insertion Sort");
            // 执行一次插入排序
            sort(original, 0, i + 1);
        } else {
            // 归并排序
            System.out.println("Merge Sort");
            int k = 1;
            boolean flag = false;
            while (!flag) {
                flag = Arrays.equals(original, current);  // 比较当前序列和目标序列
                k = k * 2;
                // 合并相邻的子数组
                for (int p = 0; p < n / k; p++) {
                    sort(original, p * k, (p + 1) * k - 1);
                }
                // 处理剩余不足一个组大小的元素
                sort(original, n / k * k, n - 1);
            }
        }

        // 输出排序后的结果
        for (int k = 0; k < n - 1; k++) {
            System.out.print(original[k] + " ");
        }
        System.out.println(original[n - 1]);
    }

    // 自定义排序函数：用于插入排序和归并排序
    public static void sort(int[] a, int from, int to) {
        for (int i = from; i < to; i++) {
            for (int j = i + 1; j <= to; j++) {
                if (a[j] < a[i]) {  // 升序排序
                    int temp = a[i];
                    a[i] = a[j];
                    a[j] = temp;
                }
            }
        }
    }
}

```



### 1036.**跟奥巴马一起编程**

```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        int n = in.nextInt();
        char symbol = in.next().charAt(0);
        int rows = 0;
        if(n % 2 == 0)    rows = n / 2;
        else rows = n/2 + 1;
        
        StringBuilder s1=new StringBuilder();
        StringBuilder s2=new StringBuilder();
        for (int i = 0; i <n ; i++) {
            s1.append(symbol);
        }
        
        int k=0;
        for (int i = 0; i < n; i++) {
            k++;
            if(k==1||k==n)    s2.append(symbol);
            else    s2.append(" ");
        }
        
        int seq=0;
        for (int i = 0; i < rows; i++) {
            seq++;
            if(seq == 1 || seq == rows)    System.out.println(s1);
            else    System.out.println(s2);
        }

    }
}
```



### 1037.**在霍格沃茨找零钱**

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
        String[] str = ins.readLine().split(" ");
        String[] payable = str[0].split("\\.");
        String[] render = str[1].split("\\.");

        long payableTotalKunt = Long.parseLong(payable[0]) * 17 * 29 +
               (long) Integer.parseInt(payable[1]) * 29 + (long) Integer.parseInt(payable[2]);
        long renderTotalKunt = Long.parseLong(render[0]) * 17 * 29 +
               (long) Integer.parseInt(render[1]) * 29 + (long) Integer.parseInt(render[2]);

        boolean engough = true;
        if(renderTotalKunt < payableTotalKunt)  engough = false;

        long change = renderTotalKunt - payableTotalKunt;
        if(engough) System.out.print(change / (17*29) + "." + (change / 29) % 17  + "." + change % 29);
        else{
            change = -change;
            System.out.print("-" + change / (17*29) + "." + (change / 29) % 17 + "." + change % 29);
        }
    }
}

```



### 1038.**统计同成绩学生**

```java
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        String[] s = ins.readLine().split(" ");
        int[] grade = new int[n];
        for(int i = 0;i < n;i++){
            grade[i] = Integer.parseInt(s[i]);
        }
        in.nextToken();
        int k = (int) in.nval;
        int[] SearchGrade = new int[k];
        int[] frequence = new int[k];
        for(int i = 0;i < k;i++){
            in.nextToken();
            int temp = (int) in.nval;
            SearchGrade[i] = temp;
        }

        for(int i = 0;i < n;i++){
            for(int j = 0;j < k;j++){
                if(SearchGrade[j] == grade[i])    frequence[j]++;
            }
        }
        for(int i = 0;i < k;i++){
            if(i != k-1)    out.print(frequence[i] + " ");
            else    out.print(frequence[i]);
        }
        out.flush();
    }
}
// 测试点3超时
```



### 1039.**到底买不买**

```java
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        char[] cur = ins.readLine().toCharArray();
        char[] target = ins.readLine().toCharArray();

        int count = 0;
        for(int i = 0;i < target.length;i++){
            for(int j = 0;j < cur.length;j++){
                if(target[i] == cur[j]){
                    count++;
                    cur[j] = ' ';
                    break;
                }
            }
        }

        if(count == target.length){
            out.print("Yes" + " " + (cur.length - count));
        }else{
            out.print("No" + " " + (target.length - count));
        }
        out.flush();
    }
}
```



### 1040.**有几个PAT** 

```java
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        char[] pat = ins.readLine().toCharArray();
        long count = 0;
        int countP = 0;
        int countT = 0;
        for(int i = 0;i < pat.length;i++){
            if(pat[i] == 'T')    countT++;
        }
        for(int i = 0;i < pat.length;i++){
            if(pat[i] == 'P')    countP++;
            if(pat[i] == 'T')    countT--;
            if(pat[i] == 'A')    count += countP * countT;
        }
        System.out.print(count % 1000000007);
    }
}

// 扫描每一个A，能组成PAT的个数即为前面P的个数和后面T的个数相乘
```



### 1041.**考试座位号**

```java
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);

    static class Student{
        String number;
        int TestSeat;
        int FinalSeat;

        Student(String number,int TestSeat,int FinalSeat){
            this.number = number;
            this.TestSeat = TestSeat;
            this.FinalSeat = FinalSeat;
        }
    }

    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        Student[] student = new Student[n];
        for(int i = 0;i < n;i++){
            String[] s = ins.readLine().split(" ");
            student[i] = new Student(s[0],Integer.parseInt(s[1]),Integer.parseInt(s[2]));
        }

        in.nextToken();
        int m = (int) in.nval;
        for(int i = 0;i < m;i++){
            in.nextToken();
            int testNumber = (int) in.nval;
            for(int j = 0;j < n;j++){
                if(student[j].TestSeat == testNumber)    out.println(student[j].number + " " + student[j].FinalSeat);
            }
        }
        out.flush();
    }
}
```



### 1042.**字符统计**

```java
import java.io.*;
import java.util.HashMap;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        char[] ch = ins.readLine().toLowerCase().toCharArray();	// 字母全部转小写
        HashMap<Character,Integer> map = new HashMap<>();

        for(int i = 0;i < ch.length;i++){
            if(ch[i] >= 'a' && ch[i] <='z'){	// 字母写入map
                if(!map.containsKey(ch[i]))    map.put(ch[i],1);	// 第一次写入
                else map.put(ch[i],map.get(ch[i]) + 1);			
            }
        }
        char mostLetter = 'z';	// 出现最多次的字母
        int count = 0;			// 出现次数
        for(Character c : map.keySet()){
            if(count < map.get(c)){	// 更新最大次数
                mostLetter = c;
                count = map.get(c);
            }
            if(count == map.get(c)){	// 相同最大次数，更新字母为更小的字母
                if(c < mostLetter)  mostLetter = c;
            }
        }
        out.print(mostLetter + " " + count);
        out.flush();
    }
}
```



### 1043.**输出PATest**

```java
import java.io.*;
import java.util.HashMap;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        String s = ins.readLine();
        char[] ch = {'P','A','T','e','s','t'};
        int[] frequence = new int[6];   // 各字符出现频率
        // 统计各字符出现频率
        for(int i = 0;i < s.length();i++){
            if(s.charAt(i) == 'P')    frequence[0]++;
            else if(s.charAt(i) == 'A')    frequence[1]++;
            else if(s.charAt(i) == 'T')    frequence[2]++;
            else if(s.charAt(i) == 'e')    frequence[3]++;
            else if(s.charAt(i) == 's')    frequence[4]++;
            else if(s.charAt(i) == 't')    frequence[5]++;
            else continue;
        }

        int count = 0;  // 最终输出字符的总数
        for(int i = 0;i < 6;i++)    count += frequence[i];

        int flag = 0;
        while(count > 0){
            int seq = flag%6;   // 当前输出的字符次序
            if(frequence[seq] != 0){    // 当前输出字符还可以继续输出
                out.print(ch[seq]);
                frequence[seq]--;
                count--;
            }
            flag++;
        }
        out.flush();
    }
}
```



### 1044.**火星数字**

```java
import java.io.*;
import java.util.HashMap;

public class Main {
    // 创建输出流
    static PrintWriter out = new PrintWriter(System.out);
    // 创建输入流，读取用户输入
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    // 创建StreamTokenizer，用于分解输入的流
    static StreamTokenizer in = new StreamTokenizer(ins);

    public static void main(String[] args) throws IOException {
        // 定义低位月份数组
        String[] low = {"tret", "jan", "feb", "mar", "apr", "may", "jun", "jly", "aug", "sep", "oct", "nov", "dec"};
        // 定义高位月份数组
        String[] high = {"tam", "hel", "maa", "huh", "tou", "kes", "hei", "elo", "syy", "lok", "mer", "jou"};

        // 读取输入数据
        in.nextToken();  // 获取输入的第一个Token
        int n = (int) in.nval;  // 获取输入的数字（输入的行数）
        String[] s = new String[n];  // 创建一个数组存储输入的字符串

        // 读取所有的输入字符串
        for (int i = 0; i < n; i++) {
            s[i] = ins.readLine();  // 每次读取一行
        }

        // 处理每一行输入
        for (int i = 0; i < n; i++) {
            // 判断输入是否是数字，若是，则调用EarthToMars转换
            if (Character.isDigit(s[i].charAt(0))) 
                out.println(EarthToMars(Integer.parseInt(s[i]), low, high));
            else  // 否则，调用MarsToEarth转换
                out.println(MarsToEarth(s[i], low, high));
        }
        // 刷新输出流
        out.flush();
    }

    // 地球文转换为火星文的函数
    static String EarthToMars(int num, String[] low, String[] high) {
        // 如果数字小于13，直接返回低位月份
        if (num < 13) 
            return low[num];
        // 如果数字能被13整除，则返回相应的高位月份
        else if (num % 13 == 0)  
            return high[num / 13 - 1];  // 高位月份要减1，因为高位月份从1开始，数组从0开始
        else
            // 否则返回高位月份和低位月份的组合
            return high[num / 13 - 1] + " " + low[num % 13]; 
    }

    // 火星文转换为地球文的函数
    static int MarsToEarth(String s, String[] low, String[] high) {
        // 将输入的火星日期分解为两个部分
        String[] str = s.split(" ");
        
        // 如果输入只有一个部分（即仅为低位或高位月份）
        if (str.length == 1) {
            // 先检查低位月份数组
            for (int i = 0; i < low.length; i++) {
                if (s.equals(low[i]))  // 如果找到匹配的低位月份
                    return i;  // 返回对应的地球日期（低位月份直接返回索引）
            }
            // 如果没有找到低位月份，检查高位月份数组
            for (int i = 0; i < high.length; i++) {
                if (s.equals(high[i]))  // 如果找到匹配的高位月份
                    return (i + 1) * 13;  // 返回对应的地球日期（高位月份的值乘以13）
            }
        }

        // 如果输入包含两个部分，表示高位和低位月份
        int fir = 0;  // 高位月份的索引
        int sec = 0;  // 低位月份的索引

        // 查找高位月份
        for (int i = 0; i < high.length; i++) {
            if (str[0].equals(high[i])) {  // 如果找到了匹配的高位月份
                fir = i + 1;  // 将高位月份索引加1
                break;
            }
        }

        // 查找低位月份
        for (int i = 0; i < low.length; i++) {
            if (str[1].equals(low[i])) {  // 如果找到了匹配的低位月份
                sec = i;  // 保存低位月份的索引
                break;
            }
        }

        // 返回转换后的地球日期，按高位和低位计算
        return fir * 13 + sec;
    }
}

```



### 1045.**快速排序**

```java
import java.io.*;
import java.util.Arrays;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);

    public static void main(String[] args) throws IOException {
        in.nextToken();
        int n = (int) in.nval;
        String[] s = ins.readLine().split(" ");
        long[] arr = new long[n];
        for (int i = 0; i < n; i++) {
            arr[i] = Long.parseLong(s[i]);
        }

        // 复制数组并排序
        long[] sortedArr = arr.clone();  
        Arrays.sort(sortedArr);  // 对复制的数组进行排序

        StringBuilder sb = new StringBuilder();
        int count = 0;    // 主元素个数
        long max = 0;    // 原数组已检查过的元素最大值
        // 4 2 3 1 5
        // 1 2 3 4 5

        // 遍历原数组，找到主元素
        for (int i = 0; i < n; i++) {
            if (arr[i] == sortedArr[i] && arr[i] > max) {  // 主元素满足条件
                if (count > 0) {
                    sb.append(" ");  // 保证主元素之间有空格
                }
                sb.append(arr[i]);
                count++;
            }
            max = Math.max(max, arr[i]);  // 更新最大值
        }

        out.println(count);  // 输出主元素个数
        out.println(sb);     // 输出主元素
        out.flush();
    }
}

```



### 1046.划拳

```java
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        int[] tar1 = new int[n];
        int[] tar2 = new int[n];
        int[] cur1 = new int[n];
        int[] cur2 = new int[n];
        
        for(int i = 0;i < n;i++){
            String[] s = ins.readLine().split(" ");
            tar1[i] = Integer.parseInt(s[0]);
            cur1[i] = Integer.parseInt(s[1]);
            tar2[i] = Integer.parseInt(s[2]);
            cur2[i] = Integer.parseInt(s[3]);
        }

        int win1 = 0,win2 = 0;
        for(int i = 0;i < n;i++){
            int target = tar1[i] + tar2[i];
            if(cur1[i] == target && cur2[i] != target)    win1++;
            else if(cur1[i] != target && cur2[i] == target)    win2++;
            else continue;
        }

        out.print(win2 + " " + win1);
        out.flush();
    }
}
```



### 1047.**编程团体赛**

```java
import java.io.*;
import java.util.HashMap;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);

    static class Team{
        int number; // 队伍编号
        int score;  // 总成绩

        Team(int number,int score){
            this.number = number;
            this.score = score;
        }
    }

    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        Team[] team = new Team[n];
        for(int i = 0;i < n;i++){
            String[] s = ins.readLine().split(" ");
            String[] teamInfo = s[0].split("\\-");
            team[i] = new Team(Integer.parseInt(teamInfo[0]),Integer.parseInt(s[1]));
        }
        
        HashMap<Integer,Integer> map = new HashMap<>(); // 队伍编号和总成绩映射
        for(int i = 0;i < n;i++){
            if(!map.containsKey(team[i].number))    map.put(team[i].number,team[i].score);
            else    map.put(team[i].number,map.get(team[i].number) + team[i].score);
        }
        int championTeam = 0;
        int maxScore = 0;
        for(Integer i : map.keySet()){
            if(maxScore < map.get(i)){
                maxScore = map.get(i);
                championTeam = i;
            }
        }
        out.print(championTeam + " " + maxScore);
        out.flush();
    }
}
```



### 1048.**数字加密**

```java
import java.io.*;
import java.util.Arrays;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
        String[] s = ins.readLine().split(" ");
        char[] digit = {'0','1','2','3','4','5','6','7','8','9','J','Q','K'};
        int l1 = s[0].length();
        int l2 = s[1].length();
        int n = Math.max(l1,l2);
        int[] secretCons = new int[n];
        int[] targetDigit = new int[n];
        char[] ans = new char[n];

        for(int i = 0;i < l1;i++){
            secretCons[i] = Integer.parseInt(String.valueOf(s[0].charAt(i)));
        }
        for(int i = 0;i < l2;i++){
            targetDigit[i] = Integer.parseInt(String.valueOf(s[1].charAt(i)));
        }
        reverse(secretCons,0,l1 - 1);
        reverse(targetDigit,0,l2 - 1);

        int i = 0;
        for(i = 0;i < Math.min(secretCons.length,targetDigit.length);i++){
            if((i+1) % 2 == 1){
                int index = (secretCons[i] + targetDigit[i]) % 13;
                ans[i] = digit[index];
            }else{
                int temp = targetDigit[i] - secretCons[i];
                if(temp < 0)    temp += 10;
                ans[i] = digit[temp];
            }
        }

         // 不能直接将较长串剩余部分直接加到ans因为偶数位B为0时应当取A+10的结果
        for(i = ans.length - 1;i >= 0;i--)    System.out.print(ans[i]);
    }

    static void reverse(int[] arr,int start,int end){
        while(start < end){
            int temp = arr[start];
            arr[start] = arr[end];
            arr[end] = temp;
            start++;
            end--;
        }
    }
}
```



### 1049.**数列的片段和**

```java
import java.io.*;
import java.text.DecimalFormat;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        String[] s = ins.readLine().split(" ");
        double[] num = new double[n];
        DecimalFormat df = new DecimalFormat("0.00");
        for(int i = 0;i < n;i++){
            num[i] = Double.parseDouble(s[i]);
        }

        double sum = 0;
        // 每个位置(从1开始)的数字加和次数为从n-i+1加到n
        for(int i = 0;i < n;i++){
            sum += num[i] * (n-i) * (i+1);
        }
        System.out.print(df.format(sum));
//	第三个测试点不过应当是因为样例数列太长导致double加和时出现精度误差，替换成下列注释就通过了
//        long sum = 0;
//        for(int i = 0;i < n;i++){
//            sum += (long)(num[i] * 1000) * (n-i) * (i+1);
//        }
//        System.out.printf(df.format(sum/1000.0));
    }
}


```



### 1050.**螺旋矩阵**

```java
import java.io.*;
import java.util.Arrays;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);

    public static void main(String[] args) throws IOException {
        // 读取 N 和待填充的数字
        in.nextToken();
        int n = (int) in.nval;
        int[] num = new int[n];
        
        for (int i = 0; i < n; i++) {
            in.nextToken();
            num[i] = (int) in.nval;
        }

        // 将数字按非递增排序
        Arrays.sort(num);
        reverse(num, 0, n - 1);

        // 计算合适的行列数
        int rows = n;
        int cols = 1;
        int min = n - 1;
        for(int i = n;i >= Math.sqrt(n);i--){    // rows ≥ cols(如果能开方，row最小是平方根)
            if(n % i == 0){
                if(i - (n/i) < min){    // rows−cols 最小。
                    rows = i;
                    cols = n/i;
                    min = rows - cols;
                }
            }
        }

        // 初始化矩阵
        int[][] matrix = new int[rows][cols];
        int top = 0, bottom = rows - 1, left = 0, right = cols - 1;
        int index = 0;

        // 填充螺旋矩阵
        while (top <= bottom && left <= right) {
            for (int i = left; i <= right; i++) matrix[top][i] = num[index++];
            top++;
            for (int i = top; i <= bottom; i++) matrix[i][right] = num[index++];
            right--;
            if (top <= bottom) {    // top++可能超过bottom
                for (int i = right; i >= left; i--) matrix[bottom][i] = num[index++];
                bottom--;
            }
            if (left <= right) {    // left++后可能超过right
                for (int i = bottom; i >= top; i--) matrix[i][left] = num[index++];
                left++;
            }
        }

        // 输出矩阵
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (j == cols - 1) out.print(matrix[i][j]);
                else out.print(matrix[i][j] + " ");
            }
            out.println();
        }
        out.flush();
    }

    // 数组反转函数
    static void reverse(int[] num, int start, int end) {
        while (start < end) {
            int temp = num[start];
            num[start] = num[end];
            num[end] = temp;
            start++;
            end--;
        }
    }
}
```





## 51~75

### 1051.**复数乘法**

```java
import java.io.*;

public class Main{

    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        double r1 = in.nval;
        in.nextToken();
        double p1 = in.nval;
        in.nextToken();
        double r2 = in.nval;
        in.nextToken();
        double p2 = in.nval;

        double a = (Math.cos(p1) * Math.cos(p2) - Math.sin(p1) * Math.sin(p2)) * r1 * r2;
        double b = (Math.cos(p1) * Math.sin(p2) + Math.cos(p2) * Math.sin(p1)) * r1 * r2;
        
        // 四舍五入在负数接近0时要把舍入后会把负号保留，所以单独去掉
//        double m = -0.004,n = 0.004;
//        System.out.printf("%.2f+%.2fi",m,n);
        if(a + 0.005 > 0 && a <= 0) a = 0.00;
        if(b + 0.005 > 0 && b <= 0) b = 0.00;
        if(b >= 0)    System.out.printf("%.2f+%.2fi",a,b);
        else    System.out.printf("%.2f-%.2fi",a,Math.abs(b));
    }
}
```



### 1052.**卖个萌**

```java
import java.io.*;

public class Main{
    static BufferedWriter out;

    static {
        try {
            out = new BufferedWriter(new OutputStreamWriter(System.out,"GBK"));
        } catch (UnsupportedEncodingException e) {
            throw new RuntimeException(e);
        }
    }

    static BufferedReader ins;

    static {
        try {
            ins = new BufferedReader(new InputStreamReader(System.in,"GBK"));
        } catch (UnsupportedEncodingException e) {
            throw new RuntimeException(e);
        }
    }
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        String[] hand = getEmotionArr(ins.readLine()).split(" ");
        String[] eye = getEmotionArr(ins.readLine()).split(" ");
        String[] mouth = getEmotionArr(ins.readLine()).split(" ");

        in.nextToken();
        int n = (int) in.nval;
        for(int i = 0;i < n;i++){
            String[] s = ins.readLine().split(" ");
            int[] seq = new int[s.length];
            for(int j = 0;j < seq.length;j++){
                seq[j] = Integer.parseInt(s[j]);
            }
            if(seq[0] < 1 || seq[0] > hand.length ||
                    seq[4] < 1 || seq[4] > hand.length ||
                    seq[1] < 1 || seq[1] > eye.length ||
                    seq[3] < 1 || seq[3] > eye.length ||
                    seq[2] < 1 || seq[2] > mouth.length)   // 序号非法
                out.write("Are you kidding me? " + "@" + "\\" + "/@\n");
            else{
                out.write(hand[seq[0]-1] + "("    // 左手
                        + eye[seq[1]-1] + mouth[seq[2]-1] + eye[seq[3]-1] + // 左眼 口 右眼
                        ")" + hand[seq[4]-1] + "\n");      // 右手
            }
            out.flush();
        }
    }

    // 将[]的字符串取出来（可能存在不在中括号的字符串）
    static String getEmotionArr(String s){
        StringBuilder sb = new StringBuilder();
        for(int i = 0;i < s.length();i++){
            if(s.charAt(i) == '['){     // 扫描到左中括号
                int j = i+1;
                while(s.charAt(j) != ']'){  // 将右中括号前的字符串直接拼接
                    sb.append(s.charAt(j));
                    j++;
                }
                sb.append(" "); // 各个字符串用空格相隔
            }
        }
        return sb.toString();
    }
}
// 傻鸟PAT这题用Java和Python3不改字符集都没办法过第一个和第二个测试用例
// Java输出和输入的默认字符集（字符编码）和C/C++的不一样，出题人没有考虑这一点！必须更换成GBK编码才可以全部AC！
```



### 1053.**住房空置率**

```java
import java.io.*;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        in.nextToken();
        double e = in.nval;
        in.nextToken();
        int d = (int) in.nval;

        int mayEmpty = 0;
        int empty = 0;

        for(int i = 0;i < n;i++){
            in.nextToken();
            int k = (int) in.nval;
            int lowdays = 0;    // 用电量低于低电量阈值e的天数
            for(int j = 0;j < k;j++){
                in.nextToken();
                double temp = in.nval;
                if(temp < e)    lowdays++;
            }
            if(lowdays > k/2)   mayEmpty++;
            if(lowdays > k/2 && k > d){
                empty++;
                mayEmpty--;
            }

        }
        System.out.printf("%.1f%% %.1f%%",mayEmpty*1.0/n*100,empty*1.0/n*100);
    }
}
```



### 1054.**求平均值**

```java
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        String[] str = ins.readLine().split(" ");
        int legalNums = 0;    // 合法输入数
        double sum = 0;    // 和
        double avg = 0;    // 平均值
        for(int i = 0;i < n;i++){
            String s = str[i];
            boolean flag = false;    // 当前字符串合法性
            if (s.matches("^-?\\d+(\\.\\d{0,2})?$"))    flag = true;   // 允许结尾带有点（例如123.）---> 测试点3
            if(flag){   // 当前字符串是数字则进一步检验合法性
                if(Double.parseDouble(s) < -1000 || Double.parseDouble(s) > 1000)    flag = false;
                if(s.indexOf(".") != -1){   // 当前字符串有小数点，进一步检查小数点后位数
                    int index = s.indexOf(".")+1;
                    String safterDot = s.substring(index);
                    if(safterDot.length() > 2) flag = false;   // 小数点后位数超过2位
                }
            }
            if(!flag){
                out.printf("ERROR: %s is not a legal number\n",s);
            }else{
                legalNums++;
                sum += Double.parseDouble(s);
            }
        }
        if(legalNums == 0)    out.printf("The average of %d numbers is Undefined",legalNums);
        else{
            avg = sum / legalNums;
            if (legalNums == 1) out.printf("The average of 1 number is %.2f", avg); // 1个数据number为单数 ---> 测试点2
            else  out.printf("The average of %d numbers is %.2f", legalNums, avg);
        }
        out.flush();
    }
}
```

#### 正则表达式：

##### 1 字符类

字符类用于匹配字符集中的任意一个字符。

- `[abc]`：匹配字符 `a`、`b` 或 `c` 中的任意一个字符。
- `[^abc]`：匹配除字符 `a`、`b`、`c` 之外的任意字符（取反）。
- `[0-9]`：匹配任何数字字符（等价于 `\\d`）。
- `[a-z]`：匹配小写字母。
- `[A-Z]`：匹配大写字母。
- `[a-zA-Z]`：匹配所有字母。
- `[0-9a-zA-Z]`：匹配任何数字和字母。

##### 2 预定义字符类

一些常见的字符类：

- `\\d`：匹配任何数字，等价于 `[0-9]`。
- `\\D`：匹配任何非数字字符，等价于 `[^0-9]`。
- `\\w`：匹配任何字母、数字和下划线，等价于 `[a-zA-Z0-9_]`。
- `\\W`：匹配任何非字母、非数字、非下划线字符。
- `\\s`：匹配任何空白字符（包括空格、制表符、换行符等）。
- `\\S`：匹配任何非空白字符。

##### 3 量词

量词指定一个字符或一个子模式的重复次数。

- `*`：匹配前面的字符零次或多次。例如，`a*` 匹配 `a`、`aa`、`aaa` 等。
- `+`：匹配前面的字符一次或多次。例如，`a+` 匹配 `a`、`aa`、`aaa` 等，但不匹配空字符串。
- `?`：匹配前面的字符零次或一次。例如，`a?` 匹配空字符串或单个字符 `a`。
- `{n}`：匹配前面的字符恰好 n 次。例如，`a{3}` 匹配 `aaa`。
- `{n,}`：匹配前面的字符至少 n 次。例如，`a{2,}` 匹配 `aa`、`aaa`、`aaaa` 等。
- `{n,m}`：匹配前面的字符至少 n 次，至多 m 次。例如，`a{2,4}` 匹配 `aa`、`aaa`、`aaaa`。

##### 4 边界匹配符

边界匹配符用于匹配字符串的位置，而不是字符。

- `^`：匹配字符串的开头。例如，`^abc` 匹配以 `abc` 开头的字符串。
- `$`：匹配字符串的结尾。例如，`abc$` 匹配以 `abc` 结尾的字符串。
- `\\b`：匹配单词边界，确保字符前后是空格或其他非单词字符。
- `\\B`：匹配非单词边界。

##### 5 分组与捕获

分组用于将多个字符模式组合成一个单元，并且可以对其进行引用或捕获。

- `()`：将字符放在括号内表示分组。例如，`(abc)+` 匹配 `abc`、`abcabc` 等。
- `\\1`、`\\2` 等：引用之前的捕获组。例如，`(\\d+)\\1` 匹配两个相同的数字。

##### 6 或运算符

- `|`：表示“或”的关系。例如，`abc|def` 匹配 `abc` 或 `def`。

##### 7 转义字符

正则表达式中的某些字符具有特殊意义（如 `.`、`*`、`+` 等）。如果你想要匹配这些字符本身，需要使用转义字符 `\\`。

- `\\.`：匹配字符 `.`。
- `\\*`：匹配字符 `*`。
- `\\+`：匹配字符 `+`。

##### 上述代码示例：

`s.matches("^-?\\d+(\\.\\d{0,2})?$")` 

1. `^` 表示字符串匹配开始，`$` 表示字符串结束匹配
2. `-?` 匹配一个 **可选的负号**。`-` 是字面字符，`?` 表示它可以出现零次或一次，所以字符串可以是正数或者负数。
3. `\\d+`
   - `\\d` 是匹配 **数字**（即 0-9）。由于反斜杠在正则表达式中有特殊意义，所以需要用两个反斜杠 `\\` 来转义。
   - `+` 表示 **一个或多个数字**。因此，`\\d+` 匹配一个或多个数字，保证数字部分至少有一个数字。
4. `(\\.\\d{0,2})?`
   + `(` 和 `)` 是 **分组**，用于将小数部分作为一个单元来进行匹配。
   + `\\.` 匹配字面字符 **点号（.）**，由于点号在正则表达式中有特殊含义，所以需要用 `\\` 转义。
   + `\\d{0,2}` 匹配 **0到2位数字**，即小数点后最多可以有两位数字。 `{0,2}` 是量词，表示数字部分可以有0到2位。
     - `0`：小数部分可以没有数字。
     - `2`：小数部分最多可以有两位数字。
   + `?` 在整个小数部分分组外面，表示前面的分组 **小数部分是可选的**。这意味着字符串可以没有小数部分，或者小数部分最多有两位数字。



### 1055.**集体照**

```java
import java.io.*;
import java.util.Arrays;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);

    static class Person implements Comparable<Person>{
        String name;
        int height;

        Person(String name,int height){
            this.name = name;
            this.height = height;
        }

        @Override
        public int compareTo(Person o){
            if(this.height != o.height)    return o.height - this.height;
            return this.name.compareTo(o.name);
        }
    }
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        in.nextToken();
        int k = (int) in.nval;
        Person[] p = new Person[n];
        for(int i = 0;i < n;i++){
            String[] s = ins.readLine().split(" ");
            p[i] = new Person(s[0],Integer.parseInt(s[1]));
        }

        Arrays.sort(p);
        //先处理最后一排
        int basicRowCount = n/k;
        int redundant = n%k;
        int end = basicRowCount + redundant - 1;
        int start = 0;
        RowSeat(p,start,end);
        // 输出最后排
        for(int i = start;i < end;i++){
            out.print(p[i].name + " ");
        }
        out.println(p[end].name);
        // 处理剩余排并输出
        start = end + 1;
        end = start + basicRowCount - 1;
        while(end < p.length){
            RowSeat(p,start,end);
            for(int i = start;i < end;i++){
                out.print(p[i].name + " ");
            }
            out.println(p[end].name);
            start = end + 1;
            end = start + basicRowCount - 1;
        }
        out.flush();
    }
    // 每排左右交替排序
    static void RowSeat(Person[] p,int start,int end){
        Person[] temp = new Person[end - start + 1];
        int index = 0;
        for(int i = start;i < end + 1;i++){
            temp[index++] = p[i];
        }

        int pivot = 0;    // 最高的人所在位置为枢轴
        if(temp.length % 2 == 0)    pivot = (end - start)/2 + 1 + start;
        else    pivot = (end - start)/2  + start;

        int offset = 1;     // 相对于枢轴偏移量
        int flag = 0;       // 偶数向左偏移，奇数向右偏移
        index = 0;          // 当前位置人在temp下标
        p[pivot] = temp[index++];   // 确定枢轴位置
        while(flag % 2 == 0 && pivot-offset >= start || flag % 2 == 1 && pivot+offset <= end){
            if(flag % 2 == 0)    p[pivot - offset] = temp[index++];
            else{
                p[pivot + offset] = temp[index++];
                offset++;   // 左右均偏移后偏移量加1
            }
            flag++; // 更新偏移方向
        }
    }
}
```



### 1056.**组合数的和**

```java
import java.io.*;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        int[] nums = new int[n];
        for(int i = 0;i < n;i++){
            in.nextToken();
            nums[i] = (int) in.nval;
        }
        int sum = 0;
        for(int i = 0;i < n;i++){
            for(int j = 0;j < n;j++){
                if(nums[i] != nums[j])    sum += nums[i]*10+nums[j];
            }
        }
        System.out.print(sum);
    }
}
```



### 1057.**数零壹**

```java
import java.io.*;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    public static void main(String[] args) throws IOException{
        String s = ins.readLine().toLowerCase();
        int sum = 0;
        for(int i = 0;i < s.length();i++){
            if(s.charAt(i) >= 'a' && s.charAt(i) <= 'z')    sum += s.charAt(i) - 'a' + 1;
        }
        String ans = Integer.toString(sum,2);
        int oneCount = 0;
        int zeroCount = 0;
        for(int i = 0;i < ans.length();i++){
            if(ans.charAt(i) == '0')    zeroCount++;
            else    oneCount++;
        }
        if(sum == 0)    System.out.print("0 0");
        else    System.out.print(zeroCount + " " + oneCount);
    }
}
```



### 1058.**选择题**

```java
import java.io.*;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));

    static class Question{
        int number;  // 题目编号
        int maxScore;   // 满分值
        String correctOptionInfo;   // 正确选项信息
        int wrongCount; // 学生错误次数

        Question(int number,int maxScore,String correctOptionInfo){
            this.number = number;
            this.maxScore = maxScore;
            this.correctOptionInfo = correctOptionInfo;
        }
    }

    static class Student{
        int score;  // 学生总得分
        String[] ansSheet;  // 学生答题卡

        Student(int score,String s){
            this.score = score;
            this.ansSheet = s.split(",");
        }
    }

    public static void main(String[] args) throws IOException{
        String[] nm = ins.readLine().split(" ");
        int n = Integer.parseInt(nm[0]);
        int m = Integer.parseInt(nm[1]);
        Question[] q = new Question[m]; // 答案卡
        for(int i = 0;i < m;i++){
            String[] s = ins.readLine().split(" ",3);
            q[i] = new Question(i+1,Integer.parseInt(s[0]),s[2]);
        }

        Student[] students = new Student[n];
        boolean simple = true;  // 学生是否全对
        int maxWrongCount = 0;  // 错误题目最多次数
        // 将学生输入改成方便分隔的形式
        for(int i = 0;i < n;i++){
            String str = ins.readLine();
            StringBuilder sb = new StringBuilder();
            for (int j = 0; j < str.length(); j++) {
                if(str.charAt(j) == '(')    continue;
                else if(str.charAt(j) == ')' && j != str.length()-1){
                    sb.append(",");         // 遇到右括号拼接逗号方便学生答题卡split
                    j++;                    // j++是因为后面有空格直接跳过就行
                }
                else if(str.charAt(j) == ')' && j == str.length()-1)   j++; // 最后的右括号什么也不加
                else sb.append(str.charAt(j));
            }
            students[i] = new Student(0,sb.toString());
        }
        for (int i = 0; i < n; i++) {       // 外循环遍历每个学生
            for (int j = 0; j < m; j++) {     // 内循环遍历每道题
                if (students[i].ansSheet[j].equals(q[j].correctOptionInfo)){    // 学生答题卡该题与正确答案一致
                    students[i].score += q[j].maxScore;
                }
                else{
                    q[j].wrongCount++;
                    if(maxWrongCount < q[j].wrongCount) maxWrongCount = q[j].wrongCount;    // 更新最多错误次数
                    simple = false; 
                }
            }
        }
        // 输出
        for (int i = 0; i < n; i++) {
            System.out.println(students[i].score);
        }
        
        if(simple) System.out.print("Too simple");
        else {
            System.out.print(maxWrongCount + " ");
            for (int i = 0; i < m-1; i++) {
                if(q[i].wrongCount == maxWrongCount) System.out.print(q[i].number + " ");
            }
            if(q[m-1].wrongCount == maxWrongCount) System.out.print(q[m-1].number);
        }
    }
}
// 测试点3超时
```



### 1059.**C语言竞赛**

```java
import java.io.*;
import java.util.HashMap;
import java.util.HashSet;
import java.util.Map;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        HashMap<Integer,Integer> awardNum = new HashMap<>();    // 获奖编号和排名映射
        for(int i = 0;i < n;i++){
            in.nextToken();
            int id = (int) in.nval;
            awardNum.put(id,i+1);
        }

        in.nextToken();
        int k = (int) in.nval;
        int[] checkNum = new int[k];    // 需要检查的编号
        for(int i = 0;i < k;i++){
            in.nextToken();
            int id = (int) in.nval;
            checkNum[i] = id;
        }

        HashSet<Integer> checked = new HashSet<>();             // 记录已经领取过奖品的编号
        for(int i = 0;i < k;i++){
            if(awardNum.containsKey(checkNum[i])){              // 当前编号为获奖编号
                if(checked.contains(checkNum[i])){              // 已经领取过奖品
                    out.printf("%04d: Checked\n",checkNum[i]);
                    continue;
                }
                if(awardNum.get(checkNum[i]) == 1){             // 冠军编号
                    out.printf("%04d: Mystery Award\n",checkNum[i]);
                    checked.add(checkNum[i]);
                }else if(isPrime(awardNum.get(checkNum[i]))){   // 素数编号
                    out.printf("%04d: Minion\n",checkNum[i]);
                    checked.add(checkNum[i]);
                }else{                                          // 其他获奖编号
                    out.printf("%04d: Chocolate\n",checkNum[i]);
                    checked.add(checkNum[i]);
                }
            }else    out.printf("%04d: Are you kidding?\n",checkNum[i]);    // 不在获奖编号中
        }
        out.flush();
    }
    // 判断素数
    static boolean isPrime(int n){
        if(n <= 1)    return false;
        if(n == 2 || n == 3)    return true;
        if(n % 2 == 0 || n % 3 == 0)    return false;
        for(int i = 5;i * i <= n;i += 6){
            if(n % i == 0 || n % (i+2) == 0)    return false;
        }
        return true;
    }
}
// 测试点1，2超时
```



### 1060.**爱丁顿数**

```java
import java.io.*;
import java.util.Arrays;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        int[] dist = new int[n];
        for(int i = 0;i < n;i++){
            in.nextToken();
            dist[i] = (int) in.nval;
        }
        Arrays.sort(dist);
        reverse(dist,0,dist.length-1);

        int E = 0;
        for(int i = 0;i < n;i++){
            if(dist[i] > i+1)    E = i+1;
            else    break;
        }
        System.out.print(E);
    }

    static void reverse(int[] arr,int start,int end){
        while(start < end){
            int temp = arr[start];
            arr[start] = arr[end];
            arr[end] = temp;
            start++;
            end--;
        }
    }
}
// 测试点3超时
```

不超时版本（不排序）：

```java
import java.io.*;
import java.util.Arrays;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        int[] dist = new int[n];
        for(int i = 0;i < n;i++){
            in.nextToken();
            dist[i] = (int) in.nval;
        }

        int E = 0;
        for(E = n;E > 0;E--){
            int days = 0;    // 超过E英里的天数
            for(int j = 0;j < n;j++){
                if(dist[j] > E)    days++;
            }
            if(days >= E)    break;
        }
        System.out.print(E);
    }
}
```



### 1061.判断题

```java
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        in.nextToken();
        int m = (int) in.nval;
        int[] score = new int[m];
        int[] ans = new int[m];
        for(int i = 0;i < m;i++){
            in.nextToken();
            score[i] = (int) in.nval;
        }
        for(int i = 0;i < m;i++){
            in.nextToken();
            ans[i] = (int) in.nval;
        }

        for(int i = 0;i < n;i++){
            int sum = 0;
            for(int j = 0;j < m;j++){
                in.nextToken();
                int answer = (int) in.nval;
                if(answer == ans[j]){
                    sum += score[j];
                }
            }
            out.println(sum);
        }
        out.flush();
    }
}
```



### 1062.**最简分数**

```java
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    public static void main(String[] args) throws IOException{
        String[] s = ins.readLine().split(" ");
        String[] str1 = s[0].split("/");
        String[] str2 = s[1].split("/");
        int k = Integer.parseInt(s[2]);
        int max = lcm(lcm(Integer.parseInt(str1[1]),Integer.parseInt(str2[1])),k);
        int times1 = max/Integer.parseInt(str1[1]);
        int times2 = max/Integer.parseInt(str2[1]);
        str1[0] = String.valueOf(Integer.parseInt(str1[0]) * times1);
        str2[0] = String.valueOf(Integer.parseInt(str2[0]) * times2);

        int start = Integer.parseInt(str1[0]),end = Integer.parseInt(str2[0]);
        if(start > end){    // 测试点1，没说明哪个数更大
            int temp = start;
            start = end;
            end = temp;
        }
        int times = max/k;  // 应该约分的因子
        boolean isFirstNum = true;
        for(int i = start+1;i < end;i++){
            if(i % times == 0){
                int t = i/times;
                if(gcd(t,k) != 1)    continue;  // 约分后不互质如 8/12 则跳过
                if(!isFirstNum)    out.print(" " + t + "/" +k);
                if(isFirstNum){
                    out.print(t + "/" + k);
                    isFirstNum = false;
                }
            }
        }
        out.flush();
    }
	
    // 最大公因数，辗转相除（swap把b=a换成b=a%b，循环条件b!=0,返回另一个数）
    static int gcd(int a,int b){
        while(b != 0){
            int temp = b;
            b = a % b;
            a = temp;
        }
        return a;
    }
	// 最小公倍数 ---> 两者相乘除以最大公因数
    static int lcm(int a,int b){
        return Math.abs(a*b)/gcd(a,b);
    }
}
```



### 1063.**计算谱半径**

```java
import java.io.*;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        double speRadius = 0;
        for(int i = 0;i < n;i++){
            in.nextToken();
            int re = (int) in.nval;
            in.nextToken();
            int lm = (int) in.nval;
            double module = Math.sqrt(re*re+lm*lm);
            if(module > speRadius)    speRadius = module;
        }
        System.out.printf("%.2f",speRadius);
    }
}
```



### 1064.**朋友数**

```java
import java.io.*;
import java.util.ArrayList;
import java.util.Collections;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        ArrayList<Integer> list = new ArrayList<>();    // 存放所有输入的朋友数
        for(int i = 0;i < n;i++){
            in.nextToken();
            int num = (int) in.nval;
            if(num > 0 && num < 10)    list.add(num);
            else if(num >= 10 && num < 100)    list.add(num/10 + num%10);
            else if(num >= 100 && num < 1000)    list.add(num/100 + (num%100)/10 + num%10);
            else    list.add(num/1000 + (num%1000)/100 + (num%100)/10 + num%10);
        }
        Collections.sort(list);   // 排序
        int[] friendNum = new int[list.size()]; // 存放去重过后的朋友数
        friendNum[0] = list.get(0);
        int count = list.size();                // 最终输出的朋友数数量
        int j = 1;
        for(int i = 1;i < list.size();i++){
            if(friendNum[j-1] == list.get(i)){  // 当前朋友数已存入过
                count--;                        
                continue;
            }else   friendNum[j++] = list.get(i);
        }
        System.out.println(count);
        for(int i = 0;i < count-1;i++){
            System.out.print(friendNum[i] + " ");
        }
        System.out.print(friendNum[count-1]);
    }
}
```



### 1065.**单身狗**

```java
import java.io.*;
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.HashSet;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        HashMap<String,String> couple1 = new HashMap<>();   // 夫-妻映射
        HashMap<String,String> couple2 = new HashMap<>();   // 妻-夫映射
        for(int i = 0;i < n;i++){
            String[] s = ins.readLine().split(" ");
            couple1.put(s[0],s[1]);
            couple2.put(s[1],s[0]);
        }

        in.nextToken();
        int m = (int) in.nval;
        String[] str = ins.readLine().split(" ");
        HashSet<String> guest = new HashSet<>();            // 客人集合
        for (int i = 0; i < m; i++) {
            guest.add(str[i]);
        }
        ArrayList<String> single = new ArrayList<>();       // 单身狗集合
        for(String s : guest){                              // 遍历客人集合找不到当前客人的夫或妻，当前客人为单身狗
            if(!guest.contains(couple1.get(s)) && !guest.contains(couple2.get(s)))  single.add(s);
        }
        Collections.sort(single);
        // 输出
        System.out.println(single.size());
        if(!single.isEmpty()){                              // 没有单身狗就不需要输出客人，不这样做会输出-1导致测试点1不通过
            for (int i = 0; i < single.size()-1; i++) {
                System.out.print(single.get(i) + " ");
            }
            System.out.print(single.get(single.size()-1));
        }
    }
}
// 测试点3,4超时
```



### 1066.**图像过滤**

```java
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int rows = (int) in.nval;    // m即行数
        in.nextToken();
        int cols = (int) in.nval;    // n即列数
        in.nextToken();
        int start = (int) in.nval;    // 过滤区间端点a
        in.nextToken();
        int end = (int) in.nval;    // 过滤区间端点b
        in.nextToken();
        int grayScale = (int) in.nval;    // 替换灰度值

        for(int i = 0;i < rows;i++){
            String[] s = ins.readLine().split(" ");
            for(int j = 0;j < cols;j++){
                if(Integer.parseInt(s[j]) >= start && Integer.parseInt(s[j]) <= end)    s[j] = String.valueOf(grayScale);
                if(j == cols-1)    out.printf("%03d\n",Integer.parseInt(s[j]));
                else    out.printf("%03d ",Integer.parseInt(s[j]));
            }
        }
        out.flush();
    }
}
// 测试点3 JAVA日常又双叒叕超时
```



### 1067.**试密码**

```java
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    public static void main(String[] args) throws IOException{
        String[] pwInfo = ins.readLine().split(" ");
        String correctPassword = pwInfo[0];
        int tryTimes = Integer.parseInt(pwInfo[1]);
        int count = 0;
        while(true){
            String psw = ins.readLine();
            count++;
            if(count > tryTimes){               // 首先要判断次数有没有超过，超过直接break，否则测试点4不过
                out.println("Account locked");
                break;
            }
            if(psw.equals("#"))    break;       // 然后再看是不是用户输入
            // 接下来再执行用户输入判断
            if(!psw.equals(correctPassword)){
                out.printf("Wrong password: %s\n",psw);
                continue;
            }
            if(psw.equals(correctPassword)){
                out.println("Welcome in");
                break;
            }
        }
        out.flush();
    }
}
```



### 1068.万绿丛中一点红

```java
import java.io.*;
import java.util.HashMap;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);

    // 检查当前像素与周围像素的差异是否超过TOL
    static boolean isUnique(int[][] pixelMatrix, int i, int j, int TOL) {
        int cur = pixelMatrix[i][j];
        int[] di = {-1, 1, 0, 0, -1, 1, -1, 1};  // 上下左右四个方向
        int[] dj = {0, 0, -1, 1, -1, -1, 1, 1};  // 以及四个对角方向

        for (int k = 0; k < 8; k++) {
            int ni = i + di[k], nj = j + dj[k];
            // 检查邻居位置合法性
            if (ni >= 0 && ni < pixelMatrix.length && nj >= 0 && nj < pixelMatrix[0].length) {    // 在矩阵中的邻居计算色差值
                if (Math.abs(cur - pixelMatrix[ni][nj]) <= TOL) {
                    return false;  // 如果有一个邻居的差异小于等于TOL，则不是唯一
                }
            }
        }
        return true;  // 所有邻居都满足条件，返回true
    }

    public static void main(String[] args) throws IOException {
        in.nextToken();
        int m = (int) in.nval;  // 列数
        in.nextToken();
        int n = (int) in.nval;  // 行数
        in.nextToken();
        int TOL = (int) in.nval;  // 容忍度

        int[][] pixelMatrix = new int[n][m];
        HashMap<Integer, Integer> map = new HashMap<>();
        
        // 读取像素矩阵并统计每个像素值的出现次数
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                in.nextToken();
                int pixel = (int) in.nval;
                pixelMatrix[i][j] = pixel;
                map.put(pixel, map.getOrDefault(pixel, 0) + 1);
            }
        }

        int count = 0;
        int x = 0, y = 0, ans = 0;

        // 遍历所有像素点，检查是否为唯一符合条件的点
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                int pixel = pixelMatrix[i][j];
                // 只有出现次数为1的像素才会被考虑
                if (map.get(pixel) != 1) {
                    continue;
                }
                // 检查当前点是否是符合条件的唯一点
                if (isUnique(pixelMatrix, i, j, TOL)) {
                    count++;
                    if (count == 2) {
                        System.out.print("Not Unique");
                        return;
                    }
                    x = j + 1;  // 列
                    y = i + 1;  // 行
                    ans = pixel;
                }
            }
        }

        // 输出结果
        if (count == 0) {
            System.out.print("Not Exist");
        } else {
            System.out.printf("(%d, %d): %d\n", x, y, ans);
        }
    }
}
// 测试点3 点在边缘
// 测试点4 不要用BufferedReader读数据不然必超时
// 测试点5 矩阵只有一个点
```



### 1069.**微博转发抽奖**

```java
import java.io.*;
import java.util.ArrayList;
import java.util.HashSet;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int m = (int) in.nval;
        in.nextToken();
        int n = (int) in.nval;
        in.nextToken();
        int s = (int) in.nval;

        if(m < s){               // 第一位中奖者序号超过转发总量
            System.out.print("Keep going...");
            return;
        }
        ArrayList<String> list = new ArrayList<>();    // 参与的网友
        HashSet<String> awardName = new HashSet<>();    // 已经中过奖的网友
        for(int i = 0;i < m;i++){
            list.add(ins.readLine());
        }

        for(int i = s-1;i < m;i += n){
            if(!awardName.contains(list.get(i))){   // 当前网友第一次中奖
                System.out.println(list.get(i));
            }else{                                  // 当前网友已经中过奖
                i++;                                // 取下一次序
                while(i < m){                       // 从下一次序开始找到下一个中奖者
                    if(!awardName.contains(list.get(i))){   // 找到下一个中奖者
                        System.out.println(list.get(i));
                        break;
                    }else i++;
                }
            }
            awardName.add(list.get(i));
        }
    }
}
```



### 1070.**结绳**

```java
import java.io.*;
import java.util.Arrays;
// 从最短的开始折折到最后就是最长
public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        double[] str = new double[n];
        for(int i = 0;i < n;i++){
            in.nextToken();
            str[i] = in.nval;
        }
        Arrays.sort(str);
        double ans = (str[0]+str[1])/2;
        for(int i = 2;i < n;i++){
            ans = (ans + str[i])/2;
        }
        System.out.print((int)Math.floor(ans));
    }
}
```



### 1071.**小赌怡情**

```java
import java.io.*;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int tokens = (int) in.nval;
        in.nextToken();
        int k = (int) in.nval;
        for(int i = 0;i < k;i++){
            in.nextToken();
            int n1 = (int) in.nval;
            in.nextToken();
            int b = (int) in.nval;
            in.nextToken();
            int t = (int) in.nval;
            in.nextToken();
            int n2 = (int) in.nval;
            if(tokens == 0){
                System.out.println("Game Over.");
                return;
            }
            if(t > tokens)    
                System.out.printf("Not enough tokens.  Total = %d.\n",tokens);
            else{
                if(b == 0 && n1 > n2 || b == 1 && n1 < n2){
                    tokens += t;
                    System.out.printf("Win %d!  Total = %d.\n",t,tokens);
                }
                else{
                    tokens -= t;
                    System.out.printf("Lose %d.  Total = %d.\n",t,tokens);
                }
            }
        }
    }
}
```



### 1072.**开学寄语**

```java
import java.io.*;
import java.util.HashSet;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);

    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        in.nextToken();
        int m = (int) in.nval;
        HashSet<String> set = new HashSet<>();  // 要查缴的物品编号
        String[] tarObj = ins.readLine().split(" ");
        for(int i = 0;i < m;i++){
            set.add(tarObj[i]);
        }

        int pbStu = 0;  // 问题学生数量
        int count = 0;  // 查缴物品数量
        for(int i = 0;i < n;i++){
            String[] s = ins.readLine().split(" ");
            boolean flag = false;       // 当前学生是否是问题学生
            StringBuilder sb = new StringBuilder();
            for(int j = 2;j < s.length;j++){
                if(set.contains(s[j])){
                    flag = true;
                    count++;
                    sb.append(" " + s[j]);
                }
            }
            if(flag){   // 是问题学生才输出
                pbStu++;
                out.println(s[0] + ":" + sb);
            }
        }
        out.printf("%d %d",pbStu,count);
        out.flush();
    }
}
```



### 1073.**多选题常见计分法**

```java
import java.io.*;
import java.util.*;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));

    static class Question{	// 正确答案类
        int number;  // 题目编号
        int maxScore;    // 满分值
        int correctOptionCount;    // 正确选项个数
        HashSet<String> set = new HashSet<>();        // 正确选项集合

        Question(int number,int maxScore,int correctOptionCount,String[] s){
            this.number = number;
            this.maxScore = maxScore;
            this.correctOptionCount = correctOptionCount;
            for(int i = 0;i < s.length;i++){
                set.add(s[i]);
            }
        }
    }

    static class Answer{    // 某个题的学生答案
        int count;
        HashSet<String> selectedOptions = new HashSet<>();    // 选择的答案集合

        Answer(String s){
            String[] str = s.split(" ");
            this.count = Integer.parseInt(str[0]);
            for(int i = 0;i < count;i++){
                selectedOptions.add(str[i+1]);
            }
        }
    }

    static class WrongQuestion implements Comparable<WrongQuestion>{    // 错题选项信息
        String optionInfo;
        int wrongCount;

        WrongQuestion(String optionInfo,int wrongCount){
            this.optionInfo = optionInfo;
            this.wrongCount = wrongCount;
        }

        public int compareTo(WrongQuestion o){
            if(this.wrongCount != o.wrongCount) return o.wrongCount - this.wrongCount;  // 按错误次数降序
            else return this.optionInfo.compareTo(o.optionInfo);                               // 按错误信息升序
        }
    }

    public static void main(String[] args) throws IOException{
        String[] nm = ins.readLine().split(" ");
        int n = Integer.parseInt(nm[0]);
        int m = Integer.parseInt(nm[1]);
        Question[] q = new Question[m];    // 正确答案卡
        for(int i = 0;i < m;i++){
            String[] s = ins.readLine().split(" ",4);
            q[i] = new Question(i+1,Integer.parseInt(s[0]),Integer.parseInt(s[2]),s[3].split(" "));
        }

        double[] score = new double[n];    // 学生得分
        Answer[] ans = new Answer[m];     // 每个学生的答题卡
        HashMap<String,Integer> map = new HashMap<>();    // 错误选项和次数的映射
        ArrayList<WrongQuestion> list = new ArrayList<>();    // 出错选项列表

        for(int i = 0;i < n;i++){        // 遍历每个学生
            String s = ins.readLine();
            StringBuilder sb = new StringBuilder();
            for(int j = 0;j < s.length();j++){    // 取出每个学生的答案信息并用逗号分隔
                if(s.charAt(j) == '(')    continue;
                else if(s.charAt(j) == ')' && j == s.length()-1)    j++;
                else if(s.charAt(j) == ')' && j != s.length()-1){
                    sb.append(",");
                    j++;
                }else    sb.append(s.charAt(j));
            }
            String[] str = sb.toString().split(",");    // 每个学生的答案信息
            for(int k = 0;k < m;k++)    ans[k] = new Answer(str[k]);    // 答案放到答题卡

            for(int k = 0;k < m;k++){    // 遍历答题卡的题目
                String key = "";
                if(q[k].set.containsAll(ans[k].selectedOptions)){   // 正确答案包含所有学生已选择选项
                    if(q[k].correctOptionCount == ans[k].count)     // 完全正确
                        score[i] += q[k].maxScore;
                    else{                                           // 部分正确
                        score[i] += q[k].maxScore/2.0;
                        for(String ss : q[k].set){                  // 找出学生没选对的选项
                            if(!ans[k].selectedOptions.contains(ss)){
                                key = (k+1) + "-" + ss;             // 错误选项信息如 2-e
                                map.put(key,map.getOrDefault(key,0)+1);
                            }
                        }
                    }
                }else{                                              // 学生选项有错误答案
                    for(String sss : ans[k].selectedOptions){       // 找出错误选项
                        if(!q[k].set.contains(sss)){
                            key = (k+1) + "-" + sss;
                            map.put(key,map.getOrDefault(key,0)+1);
                        }
                    }
                    for(String ss : q[k].set){                  // 找出学生没选对的选项
                        if(!ans[k].selectedOptions.contains(ss)){
                            key = (k+1) + "-" + ss;
                            map.put(key,map.getOrDefault(key,0)+1);
                        }
                    }
                }
            }
        }
        // 输出
        for (int i = 0; i < score.length; i++) {
            System.out.printf("%.1f\n",score[i]);
        }
        for(Map.Entry<String,Integer> entry : map.entrySet()){
            list.add(new WrongQuestion(entry.getKey(),entry.getValue()));
        }
        if(list.isEmpty()){
            System.out.println("Too simple");
            return;
        }
        Collections.sort(list);
        int maxWrongCount = list.get(0).wrongCount;
        for (int i = 0; i < list.size(); i++) {
            if(list.get(i).wrongCount == maxWrongCount)
                System.out.println(maxWrongCount + " " + list.get(i).optionInfo);
        }
    }
}
// 测试点4 Java日常超时，极限情况可能踩点399ms通过
```



### 1074.**宇宙无敌加法器**

```java
import java.io.*;
import java.math.BigInteger;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    public static void main(String[] args) throws IOException{
        char[] arr = ins.readLine().toCharArray();
        char[] arr1 = ins.readLine().toCharArray();
        char[] arr2 = ins.readLine().toCharArray();
        int[] base = new int[arr.length];
        int[] num1 = new int[base.length];
        int[] num2 = new int[base.length];
        int[] result = new int[base.length+1];  // 当前运算最高位可能会产生进位所以多留一位
        int i = arr.length-1;
        int j = arr1.length-1;
        int k = arr2.length-1;
        while(i>=0){
            if(arr[i] == '0')   base[i] = 10;   // 当前进制位为10
            else    base[i] = arr[i] - '0';
            if(j>=0)    num1[i] = arr1[j--] - '0';
            else    num1[i] = 0;
            if(k>=0)    num2[i] = arr2[k--] - '0';
            else    num2[i] = 0;
            i--;
        }
        // 反转数组方便从个位开始算进位
        reverse(base,0, base.length-1);
        reverse(num1,0,num1.length-1);
        reverse(num2,0, num2.length-1);

        boolean flag = false;   // 上一位加和是否进位
        for(i = 0;i < base.length;i++){
            int temp = num1[i] + num2[i];
            if(flag)    temp += 1;
            if(temp >= base[i]){
                result[i] = temp % base[i];
                flag = true;
            }else {
                result[i] = temp;
                flag = false;
            }
        }
        // 最高位产生进位
        if(flag)    result[i] = 1;
        StringBuilder sb = new StringBuilder();
        for (i = result.length-1;i >= 0;i--) {
            sb.append(result[i]);
        }
        // 这里要用BigInteger因为测试点3，4超过Long可表达的最大值
        System.out.print(new BigInteger(sb.toString()));
    }

    public static void reverse(int[] arr,int start,int end){
        while(start < end){
            int temp = arr[start];
            arr[start] = arr[end];
            arr[end] = temp;
            start++;
            end--;
        }
    }
}
```



### 1075.**链表元素分类**

```java
import java.io.*;
import java.util.ArrayList;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);

    static class Node{
        int address;
        int data;
        int next;

        Node(int address,int data,int next){
            this.address = address;
            this.data = data;
            this.next = next;
        }
    }
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int firAddr = (int) in.nval;
        in.nextToken();
        int n = (int) in.nval;
        in.nextToken();
        int k = (int) in.nval;
        Node[] nodes = new Node[1000000];
        for(int i = 0;i < n;i++){
            in.nextToken();
            int address = (int) in.nval;
            in.nextToken();
            int data = (int) in.nval;
            in.nextToken();
            int next = (int) in.nval;
            nodes[address] = new Node(address,data,next);
        }
        ArrayList<Node> list = new ArrayList<>();    // 初始链表
        ArrayList<Node> list1 = new ArrayList<>();    // 存放负数
        ArrayList<Node> list2 = new ArrayList<>();    // 存放0~K
        ArrayList<Node> list3 = new ArrayList<>();    // 存放>K
        ArrayList<Node> ans = new ArrayList<>();      // 结果链表
        int curAddr = firAddr;
        while(curAddr != -1){
            list.add(nodes[curAddr]);
            curAddr = nodes[curAddr].next;
        }
        
        for(int i = 0;i < list.size();i++){
            if(list.get(i).data < 0)    list1.add(list.get(i));
            else if(list.get(i).data >= 0 && list.get(i).data <= k)    list2.add(list.get(i));
            else    list3.add(list.get(i));
        }

        ans.addAll(list1);
        ans.addAll(list2);
        ans.addAll(list3);
        // 输出
        for (int i = 0; i < ans.size()-1; i++) {
            out.printf("%05d %d %05d\n",ans.get(i).address,ans.get(i).data,ans.get(i+1).address);
        }
        out.printf("%05d %d -1\n",ans.get(ans.size()-1).address,ans.get(ans.size()-1).data);
        out.flush();
    }
}
// 测试点5超时
```



## 76~100

### 1076.**Wifi密码**

```java
import java.io.*;
import java.util.HashMap;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        HashMap<String,Integer> map = new HashMap<>();  // 正确答案与对应密码位映射
        String pwd = "";    // wifi密码
        map.put("A",1);
        map.put("B",2);
        map.put("C",3);
        map.put("D",4);

        for(int i = 0;i < n;i++){
            String[] options = ins.readLine().split(" ");   // 每行选项信息
            for(int j = 0;j < options.length;j++){
                String[] info = options[j].split("-");      // 每个选项信息
                if(info[1].equals("T"))    pwd += map.get(info[0]); // 当前选项为正确选项
            }
        }
        System.out.print(pwd);
    }
}
```



### 1077.**互评成绩计算**

```java
import java.io.*;
import java.util.ArrayList;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        in.nextToken();
        int m = (int) in.nval;
        ArrayList<Integer> avgScore = new ArrayList<>();

        for(int i = 0;i < n;i++){
            in.nextToken();
            int g2 = (int) in.nval;
            int max = 0;        // 其他组的最高分
            int min = m;        // 其他组的最低分
            int sum = 0;        // 其他组评分和
            int count = 0;        // 有效评分组数
            for(int j = 0;j < n-1;j++){
                in.nextToken();
                int temp = (int) in.nval;
                if(temp < 0 || temp > m)    continue;    // 当前分数无效
                else{
                    count++;
                    if(temp > max)    max = temp;
                    if(temp < min)    min = temp;
                    sum += temp;
                }
            }
            double g1 = (sum-min-max) / (count-2);
            int avg = (int) ((g1+g2) / 2 + 0.5);
            avgScore.add(avg);
        }
        for(int i = 0;i < n;i++){
            System.out.println(avgScore.get(i));
        }
    }
}
```



### 1078.**字符串压缩与解压**（循环扫描连续相同元素，同时确保下标不越界）

```java
import java.io.*;

public class Main {
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    
    public static void main(String[] args) throws IOException {
        String flag = ins.readLine();
        char[] target = ins.readLine().toCharArray();
        String ans;
        if(flag.equals("C"))
            ans = Compression(target);
        else
            ans = Decompression(target);
        System.out.println(ans);
    }

    public static String Compression(char[] target) {
        StringBuilder sb = new StringBuilder();
        int n = target.length;
        for (int i = 0; i < n; i++) {
            char curr = target[i];
            int count = 1;
            // 循环判断连续字符，同时检查下标边界
            while (i + 1 < n && target[i + 1] == curr) {
                count++;
                i++;
            }
            // 如果连续重复，则先添加数字再添加字符
            if (count > 1) {
                sb.append(count);
            }
            sb.append(curr);
        }
        return sb.toString();
    }

    public static String Decompression(char[] target) {
        StringBuilder sb = new StringBuilder();
        int n = target.length;
        int i = 0;
        while (i < n) {
            // 如果当前字符为数字，则累积数字得到完整的重复次数
            if (Character.isDigit(target[i])) {
                int count = 0;
                while (i < n && Character.isDigit(target[i])) {
                    count = count * 10 + (target[i] - '0');
                    i++;
                }
                // 此时 i 指向待重复的字符
                if (i < n) {
                    char ch = target[i];
                    for (int k = 0; k < count; k++) {
                        sb.append(ch);
                    }
                    i++;
                }
            } else {
                // 非数字字符直接添加
                sb.append(target[i]);
                i++;
            }
        }
        return sb.toString();
    }
}

```



### 1079.**延迟的回文数**

```java
import java.math.BigInteger;
import java.util.Scanner;
import java.util.Stack;

public class Main{
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        String target = in.next();
        if(target.equals(getReverseNum(target))){  // 本身是回文数
            System.out.printf("%s is a palindromic number.",target);

        }else{
            BigInteger b1 = new BigInteger(target);
            for(int i = 0;i < 10;i++){
                BigInteger b2 = new BigInteger(getReverseNum(String.valueOf(b1)));  // 取逆转数
                String res = String.valueOf(b1.add(b2));                            // 两者相加
                System.out.println(b1 + " + " + b2 + " = " + res);
                b1 = new BigInteger(res);
                if(res.equals(getReverseNum(res))){                                 // 当前数已经是逆转数
                    System.out.printf("%s is a palindromic number.",res);
                    return;
                }
            }
            System.out.println("Not found in 10 iterations.");
        }
    }

    public static String getReverseNum(String target){
        Stack s = new Stack();
        for (int i = 0; i < target.length(); i++) {
            s.push(target.charAt(i));
        }
        String str = "";
        while(!s.isEmpty()) str += s.pop();
        return str;
    }
}
```



### 1080.**MOOC期终成绩**

```java
import java.io.*;
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.HashSet;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);

    static class Student implements Comparable<Student>{
        String name;    // 学生学号
        int Gp = -1;    // 编程成绩
        int Gm = -1;    // 期中成绩
        int Gf = -1;    // 期末成绩
        int G ;         // 总成绩

        Student(String name,int Gp,int Gm,int Gf){
            this.name = name;
            this.Gp = Gp;
            this.Gm = Gm;
            this.Gf = Gf;
            if(Gm > Gf){    // 计算总成绩
                G = (int)(Gm*0.4 + Gf*0.6 + 0.5);
            }else G = Gf;
        }

        public int compareTo(Student o){    // 确定排序规则
            if(o.G != this.G)   return o.G - this.G;
            else return this.name.compareTo(o.name);
        }
    }
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int p = (int) in.nval;
        in.nextToken();
        int m = (int) in.nval;
        in.nextToken();
        int n = (int) in.nval;
        HashMap<String,Integer> Pmap = new HashMap<>();     // 学生学号和编程成绩映射
        HashMap<String,Integer> Mmap = new HashMap<>();     // 学生学号和期中成绩映射
        HashMap<String,Integer> Nmap = new HashMap<>();     // 学生成绩和期末成绩映射
        HashSet<String> set = new HashSet<>();              // 学生学号集合
        // 录入学生成绩到对应映射
        for(int i = 0;i < p;i++){
            String[] s1 = ins.readLine().split(" ");
            Pmap.put(s1[0],Integer.parseInt(s1[1]));
            set.add(s1[0]);
        }
        for(int i = 0;i < m;i++){
            String[] s2 = ins.readLine().split(" ");
            Mmap.put(s2[0],Integer.parseInt(s2[1]));
            set.add(s2[0]);
        }
        for(int i = 0;i < n;i++){
            String[] s3 = ins.readLine().split(" ");
            Nmap.put(s3[0],Integer.parseInt(s3[1]));
            set.add(s3[0]);
        }
        // 所有学生列表
        ArrayList<Student> list = new ArrayList<>();
        // 合格学生列表
        ArrayList<Student> ans = new ArrayList<>();
        for(String s : set){
            list.add(new Student(s,Pmap.getOrDefault(s,-1),Mmap.getOrDefault(s,-1),Nmap.getOrDefault(s,-1)));
        }
        for (int i = 0; i < list.size(); i++) {
            if(list.get(i).Gp < 200)    continue;   
            if(list.get(i).Gf == -1)    continue;
            if(list.get(i).G < 60)      continue;
            ans.add(list.get(i));
        }
        Collections.sort(ans);
        for (int i = 0; i < ans.size(); i++) {
            System.out.println(ans.get(i).name + " " + ans.get(i).Gp + " " + ans.get(i).Gm + " " + ans.get(i).Gf + " " + ans.get(i).G);
        }
    }
}
//	测试点3超时
```



### 1081.**检查密码**

```java
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        for(int i = 0;i < n;i++){
            char[] pwd = ins.readLine().toCharArray();
            if(pwd.length < 6){     // 长度小于6
                out.println("Your password is tai duan le.");
                continue;
            }
            boolean isvalid = true;     // 合法性检查
            boolean haveDigit = false;  // 是否含有数字
            boolean haveLetter = false; // 是否含有字母
            for(int j = 0;j < pwd.length;j++){  // 扫描当前密码每个字符
                char cur = pwd[j];
                if(cur >= 'a' && cur <= 'z' || cur >= 'A' && cur <= 'Z')    haveLetter = true;
                else if(cur >= '0' && cur <= '9')    haveDigit = true;
                else if(cur == '.') continue;
                else isvalid = false;
            }
            if(!isvalid)    out.println("Your password is tai luan le.");
            else if(!haveDigit)    out.println("Your password needs shu zi.");
            else if(!haveLetter)    out.println("Your password needs zi mu.");
            else    out.println("Your password is wan mei.");
        }
        out.flush();
    }
}
```



### 1082.**射击比赛**

```java
import java.io.*;
import java.util.HashMap;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        int champion = 0;
        int worst = 0;
        int min = 10001;
        int max = 0;
        HashMap<Integer,Integer> map = new HashMap<>();
        for(int i = 0;i < n;i++){
            in.nextToken();
            int id = (int) in.nval;
            in.nextToken();
            int x = (int) in.nval;
            in.nextToken();
            int y = (int) in.nval;
            int dist = x*x + y*y;
            map.put(dist,id);
            if(dist > max)    max = dist;
            if(dist < min)    min = dist;
        }
        System.out.printf("%04d %04d",map.get(min),map.get(max));
    }
}
```



### 1083.**是否存在相等的差**（Map排序）

```java
import java.io.*;
import java.util.*;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        HashMap<Integer,Integer> map = new HashMap<>();     // 差值和重复次数的映射
        for(int i = 0;i < n;i++){
            in.nextToken();
            int cur = (int) in.nval;
            int diff = Math.abs(cur - i-1);
            if(!map.containsKey(diff))    map.put(diff,1);
            else    map.put(diff,map.get(diff)+1);
        }
        ArrayList<Map.Entry<Integer,Integer>> list = new ArrayList<>(map.entrySet());   
        Collections.sort(list,new Comparator<Map.Entry<Integer,Integer>>(){     // 对map中的实体进行按差值的降序
            @Override
            public int compare(Map.Entry<Integer,Integer> o1,Map.Entry<Integer,Integer> o2){
                return o2.getKey() - o1.getKey();
            }
        });
        for(int i = 0;i < list.size();i++){     // 输出差值有重复的实体
            if(list.get(i).getValue() != 1)    System.out.println(list.get(i).getKey() + " " + list.get(i).getValue());
        }
    }
}
```



### 1084.**外观数列**（类1078）

```java
import java.io.*;
import java.util.*;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int d = (int) in.nval;
        in.nextToken();
        int n = (int) in.nval;
        String ans = String.valueOf(d);
        for(int i = 2;i < n+1;i++){         // 当前为外观数列第几项
            char[] ch = ans.toCharArray();  // 当前项
            StringBuilder sb = new StringBuilder();
            for(int j = 0;j < ch.length;j++){   // 扫描当前项
                char cur = ch[j];
                int count = 1;              // 当前位重复次数
                while(j+1 < ch.length && cur == ch[j+1]){   // 从当前位置扫描到最后一个重复位置
                    count++;                
                    j++;
                }
                // 下一次循环j自动加一，所以无需对j额外处理
                sb.append(cur+""+count);    // 当前位+重复次数
            }
            ans = sb.toString();
        }
        System.out.println(ans);
    }
}
```



### 1085.**PAT单位排行**

```java
import java.io.*;
import java.util.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);

    static class School implements Comparable<School>{
        String name;    // 学习名
        int avgScore;   // 加权总分
        int candidateNum;   // 考生人数

        School(String name,int Ascore,double Bscore,double Tscore,int candidateNum){
            this.name = name;
            this.avgScore = (int)(Ascore + Bscore/1.5 + Tscore*1.5);
            this.candidateNum = candidateNum;
        }

        @Override
        public int compareTo(School o){
            if(this.avgScore != o.avgScore)    return o.avgScore - this.avgScore;   // 加权总分降序
            else if(this.candidateNum != o.candidateNum)    return this.candidateNum - o.candidateNum;  // 考生人数升序
            else    return this.name.compareTo(o.name);                             // 单位名字典序
        }
    }

    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        HashMap<String,Integer> Amap = new HashMap<>();     // 学校与甲级总分映射
        HashMap<String,Double> Bmap = new HashMap<>();      // 学校与乙级总分映射
        HashMap<String,Double> Tmap = new HashMap<>();      // 学校与顶级总分映射
        HashMap<String,Integer> num = new HashMap<>();      // 学校与考生人数映射
        for(int i = 0;i < n;i++){
            String[] info = ins.readLine().split(" ");
            String school = info[2].toLowerCase();          // 学校名
            num.put(school,num.getOrDefault(school,0)+1);
            if(info[0].charAt(0) == 'A')    Amap.put(school,Amap.getOrDefault(school,0)+Integer.parseInt(info[1]));
            else if(info[0].charAt(0) == 'B')    Bmap.put(school,Bmap.getOrDefault(school,0.0)+Double.parseDouble(info[1]));
            else    Tmap.put(school,Tmap.getOrDefault(school,0.0)+Double.parseDouble(info[1]));
        }
        ArrayList<School> list = new ArrayList<>();     // 最终输出学校列表
        for(String s : num.keySet()){
            list.add(new School(s,Amap.getOrDefault(s,0),Bmap.getOrDefault(s,0.0),Tmap.getOrDefault(s,0.0),num.get(s)));
        }
        Collections.sort(list);
        out.println(list.size());
        for(int i = 0;i < list.size();i++){
            School cur = list.get(i);
            int index = i+1;          // 学校排名（可能被共享）
            out.println(index + " " + cur.name + " " + cur.avgScore + " " + cur.candidateNum);
            while(i+1 < list.size() && cur.avgScore == list.get(i+1).avgScore){     // 找到连续的加权总分相同的学校，共享相同排名
                out.println(index + " " + list.get(i+1).name + " " + list.get(i+1).avgScore + " " + list.get(i+1).candidateNum);
                i++;
            }
        }
        out.flush();
    }
}
// 测试点4,5超时
```



### 1086.**就不告诉你**

```java
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int a = (int) in.nval;
        in.nextToken();
        int b = (int) in.nval;
        String res = String.valueOf(a*b);
        StringBuilder sb = new StringBuilder();	// 接收乘积的逆序
        for(int i = res.length()-1;i >= 0;i--){
            sb.append(res.charAt(i));
        }
        out.print(Integer.parseInt(sb.toString()));
        out.flush();
    }
}
// 测试点2,3需要将前面无关紧要的0去掉，如30*40 = 1200 则要输出21
```



### 1087.**有多少不同的值**

```java
import java.io.*;
import java.util.HashSet;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        HashSet<Integer> set = new HashSet<>();
        for(int i = 1;i <= n;i++){
            int temp1 = i/2;
            int temp2 = i/3;
            int temp3 = i/5;
            set.add(temp1+temp2+temp3);
        }
        out.print(set.size());
        out.flush();
    }
}
```



### 1088.**三人行**

```java
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int m = (int) in.nval;
        in.nextToken();
        int x = (int) in.nval;
        in.nextToken();
        int y = (int) in.nval;
        int fir = -1;    // 甲
        int sec = 0;     // 乙
        double thr = 0;    // 丙
        for(int i = 99;i > 9;i--){
            sec = (i%10)*10 + i/10;
            int diff = Math.abs(i-sec);
            if(diff*y == sec*x){
                fir = i;
                thr = diff/(x*1.0);    // 丙可能为小数（测试点4）
                break;
            }
        }
        if(fir == -1)    out.println("No Solution");
        else{
            out.print(fir);
            judge(m,(double)fir);
            judge(m,(double)(sec));
            judge(m,thr);
        }
        out.flush();
    }

    public static void judge(int m,double target){
        if(m > target)    out.print(" Gai");
        else if(m == target)    out.print(" Ping");
        else    out.print(" Cong");
    }
}
```



### 1089.**狼人杀-简单版**

```java
import java.io.*;
import java.util.ArrayList;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        int[] init = new int[n+1];
        for(int i = 1;i < n+1;i++){
            String num = ins.readLine();
            init[i] = Integer.parseInt(num);
            // 一定要用BufferReader读
        }
        // 假设i，j为狼人
        for (int i = 1; i < n; i++) {
            for (int j = i+1; j < n+1; j++) {
                ArrayList<Integer> lier = new ArrayList<>();
                for (int k = 1; k < n+1; k++) { // 扫描每个人的说法,找出说谎的人
                    if(Math.abs(init[k]) == i || Math.abs(init[k]) == j){ // 关于狼人说法
                        if(init[k] > 0)    lier.add(k); // 说狼人是好人
                    }else{                  // 关于好人说法
                        if(init[k] < 0)     lier.add(k);    // 说好人是狼人
                    }
                }
                // 说谎的人数量为2且一个是狼人一个不是
                if(lier.size() == 2 && 
                   (lier.contains(i) && !lier.contains(j) || !lier.contains(i) && lier.contains(j))){
                    System.out.print(i + " " + j);
                    return;
                }
            }
        }
        System.out.println("No Solution");
    }
}
// 本题思路简单概括为枚举狼人玩家组合的所有可能，在每个假设下找出不说实话的玩家，不说实话的玩家应当满足数量为2且一为狼人一为好人
```



### 1090.**危险品装箱**

```java
import java.io.*;
import java.util.HashMap;
import java.util.HashSet;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    public static void main(String[] args) throws IOException{
        in.nextToken();
        int n = (int) in.nval;
        in.nextToken();
        int m = (int) in.nval;
        HashMap<Integer,HashSet<Integer>> map = new HashMap<>();    // 危险品和其不相容物品的映射
        for(int i = 0;i < n;i++){
            in.nextToken();
            int fir = (int) in.nval;
            in.nextToken();
            int sec = (int) in.nval;
            // 不相容对第一个物品的映射构建
            if(map.containsKey(fir)){
                map.get(fir).add(sec);
            }else{
                HashSet<Integer> set1 = new HashSet<>();
                set1.add(sec);
                map.put(fir,set1);
            }
            // 不相容对第二个物品的映射构建
            if(map.containsKey(sec)){
                map.get(sec).add(fir);
            }else{
                HashSet<Integer> set2 = new HashSet<>();
                set2.add(fir);
                map.put(sec,set2);
            }
        }
        // 扫描每一单
        for(int i = 0;i < m;i++){
            in.nextToken();
            int count = (int) in.nval;      // 物品件数
            HashSet<Integer> set = new HashSet<>(); // 货物清单
            for (int j = 0; j < count; j++) {
                in.nextToken();
                set.add((int) in.nval);
            }

            boolean haveContraband = false; // 是否含有不相容对
            for(Integer str : map.keySet()){    // 遍历不相容对
                if(set.contains(str)){      // 含有不相容对其中一个物品
                    HashSet<Integer> temp = map.get(str);   
                    temp.retainAll(set);    // 不相容对映射和货物清单的交集
                    if(!temp.isEmpty()){
                        haveContraband = true;
                        break;
                    }
                }
            }
            if(!haveContraband)    out.println("Yes");
            else    out.println("No");
        }
        out.flush();
    }
}
```



### 1091.**N-自守数**

```java
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    public static void main(String[] args) throws IOException{
        int m = ini();
        for(int i = 0;i < m;i++){
            int k = ini();
            int temp1 = k;
            int power = 1;      // 不小于k的10的最小次方数
            while(temp1 != 0){
                power *= 10;
                temp1 /= 10;
            }
            boolean flag = false;
            int j = 1;
            for(j = 1;j < 10;j++){
                int temp2 = k*k*j;
                if(temp2 % power == k){ // 余数为自身
                    flag = true;
                    break;
                }
            }
            if(flag)    out.println(j + " " + k*k*j);
            else    out.println("No");
        }
        out.flush();
    }
}
```



### 1092.**最好吃的月饼**

```java
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    public static void main(String[] args) throws IOException{
        int n = ini();
        int m = ini();
        int[] sales = new int[n];                        // 月饼销量
        int max = 0;                                        // 月饼最大销量
        for(int i = 0;i < m;i++){
            for(int j = 0;j < n;j++){
                int salesVolume = sales[j] + ini();
                sales[j] = salesVolume;
                if(salesVolume > max)    max = salesVolume;
            }
        }
        out.println(max);
        StringBuilder res = new StringBuilder();
        for(int i = 0;i < sales.length;i++){
            if(sales[i] == max)    res.append((i+1) + " ");
        }
        out.println(res.toString().trim());
        out.flush();
        out.close();
    }
}
```



### 1093.**字符串A+B**

```java
import java.io.*;
import java.util.HashSet;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    public static void main(String[] args) throws IOException{
        char[] a = ins.readLine().toCharArray();
        char[] b = ins.readLine().toCharArray();
        HashSet<Character> set = new HashSet<>();    // 存放已出现过的字符
        StringBuilder res = new StringBuilder();
        for(int i = 0;i < a.length;i++){
            if(set.contains(a[i]))    continue;
            set.add(a[i]);
            res.append(a[i]);
        }
        for(int i = 0;i < b.length;i++){
            if(set.contains(b[i]))    continue;
            set.add(b[i]);
            res.append(b[i]);
        }
        System.out.println(res);
    }
}
```



### 1094.**谷歌的招聘**

```java
import java.io.*;
import java.math.BigInteger;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    public static void main(String[] args) throws IOException{
        int l = ini();
        int k = ini();
        String s = ins.readLine();
        for(int i = 0;i <= s.length()-k;i++){
            String str = s.substring(i,i+k);
            if(isPrime(Integer.parseInt(str))){
                System.out.print(str);
                return;
            }
        }
        System.out.print(404);
    }

    public static boolean isPrime(int n){
        if(n <= 1)    return false;
        if(n == 2 || n == 3)    return true;
        if(n % 2 == 0 || n % 3 == 0)    return false;
        for(int i = 5;i*i <= n;i += 6){
            if(n % i == 0 || n % (i+2) == 0)    return false;
        }
        return true;
    }
}
```



### 1095.**解码PAT准考证**

```java
import java.io.*;
import java.util.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    static class Candidate{
        String candidateNumber;
        String level;
        String roomNumber;
        String date;
        String number;
        int score;

        Candidate(String candidateNumber,int score){
            this.candidateNumber = candidateNumber;
            this.score = score;
            this.level = candidateNumber.charAt(0)+"";
            this.roomNumber = candidateNumber.substring(1,4);
            this.date = candidateNumber.substring(4,10);
            this.number = candidateNumber.substring(10,13);
        }
    }

    public static void main(String[] args) throws IOException{
        int n = ini();
        int m = ini();
        ArrayList<Candidate> candidates = new ArrayList<>();
        for(int i = 0;i < n;i++){
            String[] s = ins.readLine().split(" ");
            candidates.add(new Candidate(s[0],Integer.parseInt(s[1])));
        }
        for (int i = 0; i < m; i++) {
            String[] s = ins.readLine().split(" ");
            int category = Integer.parseInt(s[0]);      // 类型
            String instruction = s[1];                  // 指令
            out.printf("Case %d: %d %s\n",i+1,category,instruction);
            switch (category){
                case 1:
                    ArrayList<Candidate> res = new ArrayList<>();
                    for (int j = 0; j < candidates.size(); j++) {
                        if(candidates.get(j).level.equals(instruction)) res.add(candidates.get(j));
                    }
                    
                    if(res.isEmpty()) out.println("NA");
                    else{
                        // 排序
                        Collections.sort(res, new Comparator<Candidate>() {
                            @Override
                            public int compare(Candidate o1, Candidate o2) {
                                if(o1.score != o2.score)    return o2.score - o1.score;
                                else return o1.candidateNumber.compareTo(o2.candidateNumber);
                            }
                        });
                        // 输出
                        for (int j = 0; j < res.size(); j++) {
                            out.println(res.get(j).candidateNumber + " " + res.get(j).score);
                        }
                    }
                    break;
                case 2:
                    int count = 0;
                    int sum = 0;
                    for (int j = 0; j < candidates.size(); j++) {
                        if(candidates.get(j).roomNumber.equals(instruction)){
                            count++;
                            sum += candidates.get(j).score;
                        }
                    }
                    if(count == 0) out.println("NA");
                    else   out.println(count + " " + sum);
                    break;
                case 3:
                    HashMap<String,Integer> map = new HashMap<>();  // 考场编号和总人数映射
                    for (int j = 0; j < candidates.size(); j++) {
                        Candidate c = candidates.get(j);
                        if(c.date.equals(instruction)){
                            map.put(c.roomNumber,map.getOrDefault(c.roomNumber,0) + 1);
                        }
                    }
                    // 对map进行排序
                    ArrayList<Map.Entry<String,Integer>> ans = new ArrayList<>(map.entrySet());
                    if(ans.isEmpty()) out.println("NA");
                    else {
                        // 排序
                        Collections.sort(ans, new Comparator<Map.Entry<String, Integer>>() {
                            @Override
                            public int compare(Map.Entry<String, Integer> o1, Map.Entry<String, Integer> o2) {
                                if(o1.getValue() != o2.getValue())  return o2.getValue() - o1.getValue();
                                else return o1.getKey().compareTo(o2.getKey());
                            }
                        });
                        // 输出
                        for (int j = 0; j < ans.size(); j++) {
                            out.println(ans.get(j).getKey() + " " + ans.get(j).getValue());
                        }
                    }
            }
        }
        out.flush();
        out.close();
    }
}
// 测试点2，3,4超时
```



### 1096.**大美数**

```java
import java.io.*;
import java.util.ArrayList;
import java.util.Collections;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    public static void main(String[] args) throws IOException{
        int k = ini();
        for(int i = 0;i < k;i++){
            int num = ini();
            ArrayList<Integer> factors = findFactors(num);
            int length = factors.size();;
            if(length < 4){
                out.println("No");
                continue;
            }
            boolean isPerfect = false;
            // 暴力遍历
            for (int j = 0; j < length; j++) {
                if(isPerfect)   break;
                for (int l = j+1; l < length; l++) {
                    if(isPerfect)   break;
                    for (int m = l+1; m < length; m++) {
                        if(isPerfect)   break;
                        for (int n = m+1; n < length; n++) {
                            int sum = factors.get(j) + factors.get(l) + factors.get(m) + factors.get(n);
                            if(sum % num == 0){
                                isPerfect = true;
                                break;
                            }
                        }
                    }
                }
            }
            out.println(isPerfect ? "Yes" : "No");
        }
        out.flush();
    }
    
    // 找因子
    public static ArrayList<Integer> findFactors(int n){
        ArrayList<Integer> factors = new ArrayList<>();
        for(int i = 1;i*i <= n;i++){
            if(n % i == 0){
                factors.add(i);
                if(n != i*i)    factors.add(n/i);   // 完全平方因子只添加一次
            }
        }
        return factors;
    }
}
```



### 1097.**矩阵行平移**

```java
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    public static void main(String[] args) throws IOException{
        int n = ini();
        int k = ini();
        int x = ini();
        int shiftTimes = 0;    // 平移次数
        int[][] res = new int[n][n];    // 结果矩阵
        for(int i = 0;i < n;i++){
            if((i+1) % 2 == 0){    // 偶数行
                for(int j = 0;j < n;j++){
                    res[i][j] = ini();
                }
            }else{    // 奇数行
                shiftTimes++;
                if(shiftTimes > k)  shiftTimes = 1;
                int j = 0;
                for (j = 0; j < shiftTimes; j++) {  // 填充平移次数个的x
                    res[i][j] = x;
                }
                for(int m = 0;m < n-shiftTimes;m++){    // 填充剩下的行元素
                    res[i][j++] = ini();
                }
                for (j = 0; j < shiftTimes; j++) {      // 跳过后续读取的剩余行元素
                    ini();
                }
            }
        }
        for (int i = 0; i < n; i++) {
            int sum = 0;
            for (int j = 0; j < n; j++) {
                sum += res[j][i];
            }
            if(i == 0)  out.print(sum);
            else    out.print(" " + sum);
        }
        out.flush();
    }
}
```



### 1098.**岩洞施工**

```java
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    public static void main(String[] args) throws IOException{
        int n = ini();
        int highLowest = 1000;    // 顶部最小高度
        int lowHighest = 0;       // 底部最大高度
        for(int i = 0;i < n;i++){
            int sample = ini();
            if(sample < highLowest)    highLowest = sample;
        }
        for(int i = 0;i < n;i++){
            int sample = ini();
            if(sample > lowHighest)    lowHighest = sample;
        }
        if(highLowest > lowHighest)    out.print("Yes " + (highLowest - lowHighest));
        else    out.print("No " + (lowHighest - highLowest + 1));
        out.flush();
    }
}
```



### 1099.**性感素数**

```java
import java.io.*;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    public static void main(String[] args) throws IOException{
        int n = ini();
        if(isPrime(n)){        // 输入的素数
            if(isPrime(n-6)){
                System.out.println("Yes");
                System.out.println(n-6);
            }else if(isPrime(n+6)){
                System.out.println("Yes");
                System.out.println(n+6);
            }else{            // 不是性感素数
                System.out.println("No");
                System.out.println(findMinSexPrime(n));
            }
        }else{                // 输入的不是素数
            System.out.println("No");
            System.out.println(findMinSexPrime(n));
        }
    }

    public static boolean isPrime(int n){
        if(n < 2 || n % 2 == 0 ||n % 3 == 0)    return false;
        if(n == 2 || n == 3)    return true;
        for(int i = 5;i*i <= n;i += 6){
            if(n % i == 0 || n % (i+2) == 0)    return false;
        }
        return true;
    }

    public static int findMinSexPrime(int n){
        int res = n+1;
        while((!isPrime(res) || (!isPrime(res-6)) && !isPrime(res+6)))    res++;
        return res;
    }
}
```



### 1100.**校庆**

```java
import java.io.*;
import java.util.HashMap;
import java.util.HashSet;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }

    public static void main(String[] args) throws IOException{
        int n = ini();
        HashSet<String> alumnus = new HashSet<>();    // 校友集合
        for(int i = 0;i < n;i++){
            alumnus.add(ins.readLine());
        }
        int m = ini();
        // 最年长来访校友
        String eldest1 = "20200101";
        String eldestAlum = "";
        // 最年长来访客人
        String eldest2 = "20200101";
        String eldestGuest = "";
        int count = 0;  // 来访校友人数
        for(int i = 0;i < m;i++){
            String guest = ins.readLine();
            String birth = guest.substring(6,14);
            if(alumnus.contains(guest)){    // 来访的是校友
                count++;
                if(isBirOrderThan(birth,eldest1)){
                    eldestAlum = guest;
                    eldest1 = birth;
                }
            }
            if(count == 0){             // 目前尚未有校友来访
                if(isBirOrderThan(birth,eldest2)){
                    eldestGuest = guest;
                    eldest2 = birth;
                }
            }
        }
        out.println(count);
        if(count != 0)  out.println(eldestAlum);
        else out.println(eldestGuest);
        out.flush();
    }
    // bir1是否比bir2年长
    public static boolean isBirOrderThan(String bir1,String bir2){
        int year = Integer.parseInt(bir1.substring(0,4));
        int month = Integer.parseInt(bir1.substring(4,6));
        int day = Integer.parseInt(bir1.substring(6,8));
        int cmpYear = Integer.parseInt(bir2.substring(0,4));
        int cmpMonth = Integer.parseInt(bir2.substring(4,6));
        int cmpDay = Integer.parseInt(bir2.substring(6,8));
        return year < cmpYear || 
               year == cmpYear && month < cmpMonth || 
               year == cmpYear && month == cmpMonth && day < cmpDay;
    }
}
```



## 101~125

### 1101.**B是A的多少倍**

```java
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    public static void main(String[] args) throws IOException{
        int a = ini();
        int d = ini();
        char[] ch = String.valueOf(a).toCharArray();
        int n = ch.length;
        reverse(ch,0,n-1);
        reverse(ch,0,d-1);
        reverse(ch,d,n-1);
        double b = Double.parseDouble(new String(ch));
        double times = b/a;
        out.printf("%.2f",times);
        out.flush();
    }
    public static void reverse(char[] ch,int start,int end){
        while(start < end){
            char temp = ch[start];
            ch[start] = ch[end];
            ch[end] = temp;
            start++;
            end--;
        }
    }
}
// 解题思路与1008循环右移相同
```



### 1102.**教超冠军卷**

```java
import java.io.*;
import java.util.HashMap;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    public static void main(String[] args) throws IOException{
        int n = ini();
        HashMap<Integer,String> salesAmount = new HashMap<>();  // 销量和名称映射
        HashMap<Integer,String> salesVolume = new HashMap<>();  // 销售额和名称映射
        int maxAmount = 0;
        int maxVolume = 0;
        for(int i = 0;i < n;i++){
            String[] s = ins.readLine().split(" ");
            int amount = Integer.parseInt(s[2]);
            int volume = Integer.parseInt(s[1]) * amount;
            if(amount > maxAmount)    maxAmount = amount;
            if(volume > maxVolume)    maxVolume = volume;
            salesAmount.put(amount,s[0]);
            salesVolume.put(volume,s[0]);
        }
        out.println(salesAmount.get(maxAmount) + " " + maxAmount);
        out.println(salesVolume.get(maxVolume) + " " + maxVolume);
        out.flush();
    }
}
```



### 1103.缘分数

```java
import java.io.*;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    public static void main(String[] args) throws IOException{
        int m = ini();
        int n = ini();
        int count = 0;  // 缘分数对数
        for(int a = m;a <= n;a++){
            int b = 0;
            int c = 0;
            int diff = a*a*a - (a-1)*(a-1)*(a-1);
            int temp = (int) Math.sqrt(diff);
            if(temp * temp == diff)    c = temp;    // diff是完全平方数
            else  continue;
            int i = 2;                              // 测试点5要求从2开始而不是1，题目认为（1,1）不是缘分数
            while(i*i + (i-1)*(i-1) <= c){
                if(c == i*i + (i-1)*(i-1)){
                    b = i;
                    count++;
                    break;
                }
                i++;
            }
            if(b == 0)  continue;
            else    System.out.println(a + " " + b);
        }
        if(count == 0)    System.out.println("No Solution");
    }
}
```



### 1104.天长地久数

```java
import java.io.*;
import java.util.*;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    static class R implements Comparable<R>{    // 最终结果需要排序好在输出
        int n;
        int a;
        R(int n,int a){
            this.n = n;
            this.a = a;
        }

        public int compareTo(R o){
            if(this.n != o.n)   return this.n - o.n;
            else    return this.a - o.a;
        }
    }
    public static void main(String[] args) throws IOException{
        int N = ini();
        for(int i = 0;i < N;i++){
            int k = ini();
            int m = ini();
            int start = (int)Math.pow(10,k-1)+99;
            int end = (int)Math.pow(10,k);
            ArrayList<R> res = new ArrayList<>();
            for(int a = start;a < end;a += 100){        // 天长地久数最后两位必为99
                char[] num1 = String.valueOf(a).toCharArray();
                char[] num2 = String.valueOf(a+1).toCharArray();
                int sum1 = 0;
                int n = 0;
                for(int j = 0;j < k;j++){
                    sum1 += (num1[j] - '0');
                    n += (num2[j] - '0');
                }
                if(sum1 == m && isPrime(gcd(m,n))){
                    res.add(new R(n,a));
                }
            }
            if(res.isEmpty()){
                System.out.printf("Case %d\n",i+1);
                System.out.println("No Solution");
            }else{
                Collections.sort(res);
                for (int j = 0; j < res.size(); j++) {
                    if(j == 0)  System.out.printf("Case %d\n",i+1);
                    System.out.println(res.get(j).n + " " + res.get(j).a);
                }
            }
        }
    }
    // 最大公因数
    public static int gcd(int a,int b){
        while(b != 0){
            int temp = b;
            b = a % b;
            a = temp;
        }
        return a;
    }

    // 是否为大于2的素数
    public static boolean isPrime(int n){
        if(n <= 2)    return false;
        if(n == 3)    return true;
        if(n % 2 == 0 || n % 3 == 0)    return false;
        for(int i = 5;i*i <= n;i += 6){
            if(n % i == 0 || n % (i+2) == 0)    return false;
        }
        return true;
    }
}
```



### 1105.**链表合并**

```java
import java.io.*;
import java.util.ArrayList;
import java.util.Collections;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    static class Node{
        int address;
        int data;
        int next;

        Node(int address,int data,int next){
            this.address = address;
            this.data = data;
            this.next = next;
        }
    }
    public static void main(String[] args) throws IOException{
        int fir1 = ini();
        int fir2 = ini();
        int n = ini();
        Node[] nodes = new Node[100000];
        for(int i = 0;i < n;i++){
            int addr = ini();
            int data = ini();
            int next = ini();
            nodes[addr] = new Node(addr,data,next);
        }
        ArrayList<Node> list1 = new ArrayList<>();
        ArrayList<Node> list2 = new ArrayList<>();
        // 生成初始链表L1和L2
        while(fir1 != -1){
            list1.add(nodes[fir1]);
            fir1 = nodes[fir1].next;
        }
        while(fir2 != -1){
            list2.add(nodes[fir2]);
            fir2 = nodes[fir2].next;
        }
        // 合并两个链表
        ArrayList<Node> longList, shortList;
        if (list1.size() >= list2.size()) {
            longList = list1;
            shortList = list2;
        } else {
            longList = list2;
            shortList = list1;
        }
        Collections.reverse(shortList);
        ArrayList<Node> res = merge(longList,shortList);
        for (int i = 0; i < res.size(); i++) {
            if(i != res.size()-1)   out.printf("%05d %d %05d\n",res.get(i).address,res.get(i).data,res.get(i).next);
            else    out.printf("%05d %d -1\n",res.get(i).address,res.get(i).data);
        }
        out.flush();
    }

    // 合并两个链表，list1为长链表
    public static ArrayList<Node> merge(ArrayList<Node> longList,ArrayList<Node> shortList){
        int i = 0,j = 0;
        ArrayList<Node> res = new ArrayList<>();
        // 按顺序填充res
        while (i < longList.size() && j < shortList.size()) {
            // 添加长链表的两个节点（如果存在）
            res.add(longList.get(i++));
            if (i < longList.size()) {
                res.add(longList.get(i++));
            }
            // 添加短链表的一个节点
            res.add(shortList.get(j++));
        }
        // 追加长链表剩余节点
        while (i < longList.size()) {
            res.add(longList.get(i++));
        }
        // 调整next指针
        for (int k = 0; k < res.size() - 1; k++) {
            res.get(k).next = res.get(k+1).address;
        }
        res.get(res.size()-1).next = -1;
        return res;
    }
}
// 测试点4超时
```



### 1106.2019数列

```java
import java.io.*;
import java.util.ArrayList;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    public static void main(String[] args) throws IOException{
        int n = ini();
        ArrayList<Integer> list = new ArrayList<>();
        list.add(2);
        list.add(0);
        list.add(1);
        list.add(9);
        int i = 0;
        while(i < n-4){
            int temp = list.get(i)+list.get(i+1)+list.get(i+2)+list.get(i+3);
            list.add(temp%10);
            i++;
        }
        for(i = 0;i < n;i++){
            System.out.print(list.get(i));
        }
    }
}
```



### 1107.老鼠爱大米

```java
import java.io.*;
import java.util.ArrayList;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    public static void main(String[] args) throws IOException{
        int n = ini();
        int m = ini();
        ArrayList<Integer> res = new ArrayList<>();
        for(int i = 0;i < n;i++){
            int max = 0;
            for(int j = 0;j < m;j++){
                int cur = ini();
                if(cur > max)    max = cur;
            }
            res.add(max);
        }
        int champion = res.get(0);
        for(int i = 0;i < res.size();i++){
            int cur = res.get(i);
            if(i == res.size()-1)    System.out.print(cur + "\n");
            else    System.out.print(cur + " ");
            if(cur > champion)    champion = cur;
        }
        System.out.println(champion);
    }
}
```



### 1108.**String复读机**

```java
import java.io.*;
import java.util.HashMap;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    public static void main(String[] args) throws IOException{
        String s = ins.readLine();
        String str = "String";
        HashMap<Integer,Character> map1 = new HashMap<>();    // 下标和字符映射
        HashMap<Character,Integer> map2 = new HashMap<>();    // 字符和下标映射
        for(int i = 0;i < str.length();i++){
            map1.put(i,str.charAt(i));
            map2.put(str.charAt(i),i);
        }
        int[] count = new int[6];    // 每个目标字符出现次数
        int sum = 0;                // 所有目标字符总出现次数
        for(int i = 0;i < s.length();i++){
            char cur = s.charAt(i);
            if(map2.containsKey(cur)){
                count[map2.get(cur)]++;
                sum++;
            }
        }
        while(sum != 0){
            for(int i = 0;i < 6;i++){    // 循环输出String
                if(count[i] != 0){
                    System.out.print(map1.get(i));
                    count[i]--;
                    sum--;
                }
            }
        }
    }
}
```



### 1109.**擅长C**

```java
import java.io.*;
import java.util.ArrayList;
import java.util.HashMap;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    // 打印当前字母的目标行
    public static void printRow(char[] ch){
        for (int i = 0; i < ch.length; i++) {
            out.print(ch[i]);
        }
    }
    public static boolean isUpperLetter(char c){
        if(c >= 'A' && c <= 'Z')    return true;
        else return false;
    }
    public static void main(String[] args) throws IOException{
        HashMap<Character,char[][]> letterMap = new HashMap<>();    // 字母和对应矩阵映射
        for (int i = 0; i < 26; i++) {
            char[][] letter = new char[7][5];
            for (int j = 0; j < 7; j++) {
                letter[j] = ins.readLine().toCharArray();
            }
            letterMap.put((char)('A'+i),letter);
        }
        // 目标字符串
        char[] target = ins.readLine().toCharArray();
        String allWords = "";
        ArrayList<String> word = new ArrayList<>();
        boolean flag = false;   // 上一个字符是不是大写字母
        // 目标字符串去掉所有非大写字母，用空格隔开每个单词
        for (int i = 0; i < target.length; i++) {
            char c = target[i];
            if(!flag && !isUpperLetter(c))   continue;  // 上一个字符和当前字符都不是大写字母
            if(isUpperLetter(c)){
                allWords += c;
                flag = true;
            }else {
                if(i != target.length-1)    allWords += " ";	// 末尾不加空格
                flag = false;
            }
        }
        String[] w = allWords.split(" ");
        for (int i = 0; i < w.length; i++) {
            word.add(w[i]);
        }
        for (int i = 0; i < word.size(); i++) {     // 打印每个单词
            String curWord = word.get(i);           // 当前单词
            for (int j = 0; j < 7; j++) {           // 打印单词的第j行
                for (int k = 0; k < curWord.length(); k++) {    // 打印单词第k个字母
                    char curLetter = curWord.charAt(k); 
                    printRow(letterMap.get(curLetter)[j]);  // 打印当前字母的第j行
                    //当前字母的第j行打印完，不是最后一个字母的话加个空格
                    if(k != curWord.length()-1)     out.print(" ");
                    else    out.print("\n");
                }
                if(i != word.size()-1 && j == 6) out.println();  // 非最后一个单词要换行
            }
        }
        out.flush();
        out.close();
    }
}
//第1，2个测试点中还存在两个单词中间有多个非大写字母字符的情况。
//第4个测试点要考虑最后以大写字母结尾，要注意不要少输出。
```



### 1110.**区块反转**

```java
import java.io.*;
import java.util.ArrayList;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    static class Node{
        int address;
        int data;
        int next;

        Node(int address,int data,int next){
            this.address = address;
            this.data = data;
            this.next = next;
        }
    }
    public static void main(String[] args) throws IOException{
        int firaddr = ini();
        int n = ini();
        int k = ini();
        Node[] nodes = new Node[100000];
        for(int i = 0;i < n;i++){
            int address = ini();
            int data = ini();
            int next = ini();
            nodes[address] = new Node(address,data,next);
        }
        // 生成初始链表
        ArrayList<Node> list = new ArrayList<>();
        while(firaddr != -1){
            list.add(nodes[firaddr]);
            firaddr = nodes[firaddr].next;
        }
        n = list.size();    // 有效节点数（去掉不属于这条链表的节点） ---> 测试点6
        int redundantNodes = n % k;
        reverse(list,0,n-1);                // 反转整个链表
        reverse(list,0,redundantNodes-1);    // 反转不足k个结点的区块
        for(int i = redundantNodes;i < n;i += k){    // 反转剩余区块
            reverse(list,i,i+k-1);
        }
        for(int i = 0;i < n-1;i++){
            out.printf("%05d %d %05d\n",list.get(i).address,list.get(i).data ,list.get(i+1).address);
        }
        out.printf("%05d %d -1\n",list.get(n-1).address,list.get(n-1).data);
        out.flush();
        out.close();
    }
    // 反转链表
    public static void reverse(ArrayList<Node> list,int start,int end){
        while(start < end){
            Node temp = list.get(start);
            list.set(start,list.get(end));
            list.set(end,temp);
            start++;
            end--;
        }
    }
}
// 测试点5超时
```



### 1111.对称日

```java
import java.io.*;
import java.util.HashMap;
import java.util.Stack;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    public static void main(String[] args) throws IOException{
        int n = ini();
        String[] month = {"Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"};
        HashMap<String,Integer> monthMap = new HashMap<>(); // 月份和数字映射
        for(int i = 0;i < 12;i++){
            monthMap.put(month[i],i+1);
        }
        for(int i = 0;i < n;i++){
            String[] s = ins.readLine().split(",");
            int curYear = Integer.parseInt(s[1].trim());
            String[] str = s[0].split(" ");
            int curMonth = monthMap.get(str[0]);
            int curDay = Integer.parseInt(str[1]);
            // 格式化日期
            String date = String.format("%04d",curYear) + String.format("%02d",curMonth) + String.format("%02d",curDay);
            // 用栈判断是否为回文日期
            Stack stack = new Stack();
            for (int j = 0; j < date.length(); j++) {
                stack.push(date.charAt(j));
            }
            String reverseDate = "";
            for (int j = 0; j < date.length(); j++) {
                reverseDate += stack.pop();
            }
            if(date.equals(reverseDate))    System.out.println("Y " + date);
            else System.out.println("N " + date);
        }
    }
}
```



### 1112.**超标区间**

```java
import java.io.*;
import java.util.*;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    public static void main(String[] args) throws IOException{
        int n = ini();
        int t = ini();
        int max = 0;                            // 最大值点
        boolean flag = false;                   // 是否有超过阈值的点
        int[] overthresh = new int[n];          // 记录超过阈值的点
        for(int i = 0;i < n;i++){
            int cur = ini();
            if(cur <= t){
                if(cur > max)    max = cur;
                continue;
            }
            overthresh[i] = 1;
            flag = true;
        }
        if(!flag){
            System.out.print(max);
            return;
        }
        
        boolean isLastValid = false;    // 最后一个点是否超过阈值
        for(int i = 0;i < n-1;i++){
            if(overthresh[i] == 0)    continue;
            int start = i;
            while(i < n-1 && overthresh[i+1] != 0){ // 找出超过阈值区间的右端点
                i++;
                if(i == n-1)    isLastValid = true; // 最后一个端点是连续区间的右端点
            }
            System.out.println("[" + start + ", " + i + "]");
        }
        if(!isLastValid && overthresh[n-1] != 0) System.out.println("[" + (n-1) + ", " + (n-1) + "]");
    }
}
// 边界条件
// 当输入个数为1且输入纵坐标>阈值时
// 当所有数据都大于阈值时
```



### 1113.**钱串子的加法**

```java
import java.io.*;
import java.util.ArrayList;
import java.util.HashMap;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    public static void main(String[] args) throws IOException{
        HashMap<Integer,Character> map1 = new HashMap<>();  // 十进制和30进制映射
        HashMap<Character,Integer> map2 = new HashMap<>();  // 30进制和十进制映射
        for(int i = 0;i < 30;i++){
            if(i < 10){
                char c1 = (char)(i+'0');
                map1.put(i,c1);
                map2.put(c1,i);
            }
            else{
                char c2 = (char)((i-10)+'a');
                map1.put(i,c2);
                map2.put(c2,i);
            }
        }
        // 将两个数扩展为同样长度
        String[] num = ins.readLine().split(" ");
        ArrayList<Character> num1 = new ArrayList<>();
        ArrayList<Character> num2 = new ArrayList<>();
        ArrayList<Character> res = new ArrayList<>();
        int n = Math.max(num[0].length(),num[1].length());
        int diff1 = n - num[0].length();    // num1需要补的0个数
        int diff2 = n - num[1].length();    // num2需要补的0个数
        for (int i = 0; i < n; i++) {
            if(i < diff1) num1.add('0');
            else num1.add(num[0].charAt(i-diff1));
            if(i < diff2) num2.add('0');
            else num2.add(num[1].charAt(i-diff2));
        }
        
        int carry = 0;      // 进位位
        for(int i = n-1;i >= 0;i--){
            int temp =  map2.get(num1.get(i)) + map2.get(num2.get(i)) + carry;
            if(temp < 30){
                res.add(map1.get(temp));
                carry = 0;
            }else{
                carry = 1;
                res.add(map1.get(temp%30));
            }
        }
        // 最高位产生了进位
        if(carry == 1)  res.add('1');
        // 去掉前导0（找出第一个非0的数的下标）
        int fir = res.size()-1;
        if(res.get(fir) == '0'){
            while(fir >= 1 && res.get(fir-1) == '0')    fir--;
        }
        // 输出
        for (int i = fir; i >=0 ; i--) {
            out.print(res.get(i));
        }
        out.flush();
        out.close();
    }
}
```



### 1114.**全素日**

```java
import java.io.*;
import java.util.ArrayList;
import java.util.HashMap;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    public static void main(String[] args) throws IOException{
        String num = String.format("%08d",ini());   // 测试点2,5不是格式化的输入
        boolean isAllPrime = true;
        for(int i = 0;i < 8;i++){
            String s = num.substring(i,8);
            int cur = Integer.parseInt(s);
            if(!isPrime(cur)){
                isAllPrime = false;
                out.println(s + " No");
            }else    out.println(s + " Yes");
        }
        if(isAllPrime)    out.println("All Prime!");
        out.flush();
        out.close();
    }
    public static boolean isPrime(int n){
        if(n <= 1)    return false;
        if(n == 2 || n == 3)    return true;
        if(n % 2 == 0 || n % 3 == 0)    return false;
        for(int i = 5;i*i <= n;i += 6){
            if(n % i == 0 || n % (i+2) == 0)    return false;
        }
        return true;
    }
}
```



### 1115.**裁判机**

```java
import java.io.*;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.HashSet;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    public static void main(String[] args) throws IOException{
        int a = ini();
        int b = ini();
        int n = ini();
        int m = ini();
        int[][] matrix = new int[n][m];
        for(int i = 0;i < n;i++){
            for(int j = 0;j < m;j++){
                matrix[i][j] = ini();
            }
        }
        HashSet<Integer> occurNum = new HashSet<>();    // 已经出现过的正整数集合
        occurNum.add(a);
        occurNum.add(b);
        HashSet<Integer> diff = new HashSet<>();        // 已经出现过的差值集合
        diff.add(Math.abs(a-b));
        boolean[] isOut = new boolean[n];               // 出局标记
        for (int i = 0; i < isOut.length; i++) {
            isOut[i] = false;
        }
        int outCount = 0;                               // 出局人数

        for(int i = 0;i < m;i++){
            for(int j = 0;j < n;j++){
                if(isOut[j])    continue;   // 第j+1个人已经出局
                int cur = matrix[j][i];     // 第j+1个人的第i+1轮给出的数字
                if(!diff.contains(cur) || occurNum.contains(cur)){  // 出局判断
                    isOut[j] = true;
                    outCount++;
                    out.printf("Round #%d: %d is out.\n",i+1,j+1);
                    continue;
                }
                // 更新已经出现过的正整数集合和已经出现过的差值集合
                for(Integer ioN : occurNum){
                    int curDiff = Math.abs(cur-ioN);
                    if(!diff.contains(curDiff))    diff.add(curDiff);
                }
                occurNum.add(cur);
            }
        }
        // 所有人都出局
        if(outCount == n)   out.println("No winner.");
        else{
            out.print("Winner(s):");
            for (int i = 0; i < isOut.length; i++) {
                if(!isOut[i]) out.print(" " + (i+1));
            }
        }
        out.flush();
        out.close();
    }
}
// 测试点5超时，用数组代替set可以减少遍历时间
```



### 1116.多二了一点

```java
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    public static void main(String[] args) throws IOException{
        String num = ins.readLine();
        if(num.length() % 2 != 0){
            System.out.printf("Error: %d digit(s)",num.length());
            return;
        }
        int n = num.length()/2;
        String pre = num.substring(0,n);
        String back = num.substring(n);
        int preSum = 0;
        int backSum = 0;
        for (int i = 0; i < n; i++) {
            preSum += pre.charAt(i)-'0';
            backSum += back.charAt(i)-'0';
        }
        if(preSum == backSum-2)    out.println("Yes: " + back + " - " + pre + " = 2");
        else    out.println("No: " + back + " - " + pre + " != 2");
        out.flush();
        out.close();
    }
}
```



### 1117.**数字之王**

```java
import java.io.*;
import java.util.ArrayList;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    public static void main(String[] args) throws IOException{
        int n1 = ini();
        int n2 = ini();
        boolean flag = true;    // 当前序列是否全为1位数
        ArrayList<Integer> list = new ArrayList<>();
        for(int i = n1;i <= n2;i++){
            if(i > 9)   flag = false;
            list.add(i);
        }
        if(flag){   // 初始序列全为1位数
            System.out.println(1);
            for (int i = 0; i < list.size(); i++) {
                if(i == 0) System.out.print(list.get(i));
                else System.out.print(" " + list.get(i));
            }
            return;
        }
        // 循环至序列全为1位数，是则跳出循环，不是则进行下一轮判断
        while(!flag){
            flag = true;
            int[] count = new int[10];  // 当前序列全为1位数时，各数字出现次数
            int maxCount = 0;           // 数字之王出现次数
            for (int i = 0; i < list.size(); i++) {
                char[] cur = String.valueOf(list.get(i)).toCharArray();
                // 获得各个位置上数字的立方相乘的数字
                int temp = 1;
                for (int j = 0; j < cur.length; j++) {
                    int temp1 = (cur[j]-'0') * (cur[j]-'0') * (cur[j]-'0');
                    temp *= temp1;
                }
                // 获得立方相乘后的数字各个位置上的数字之和
                cur = String.valueOf(temp).toCharArray();
                int newNum = 0;
                for (int j = 0; j < cur.length; j++) {
                    newNum += cur[j]-'0';
                }
                // 为1位数时更新次数数组，否则标记成false进行下一次训练
                if(newNum > 9)  flag = false;
                else{
                    count[newNum]++;
                    if(count[newNum] > maxCount)    maxCount = count[newNum];
                }
                // 更新序列
                list.set(i,newNum);
            }
            // 无需进行下一次循环，则输出
            if(flag){
                out.println(maxCount);
                int index = 0;
                for (int i = 0; i < count.length; i++) {
                    if(count[i] == maxCount){
                        if(index == 0){
                            index++;
                            out.print(i);
                        }else out.print(" " + i);
                    }
                }
            }
        }
        out.flush();
        out.close();
    }
}
```



### 1118.**如需挪车请致电**

```java
import java.io.*;
import java.util.HashMap;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    public static void main(String[] args) throws IOException{
        String[] word = {"ling","yi","er","san","si","wu","liu","qi","ba","jiu"};
        HashMap<String,Integer> map = new HashMap<>();
        for(int i = 0;i < 10;i++){
            map.put(word[i],i);
        }
        String res = "";
        for(int i = 0;i < 11;i++){
            int cur = -1;   // 当前要输出的数字
            String s = ins.readLine();
            char[] ch = s.toCharArray();
            if(ch.length == 1)    cur = Integer.parseInt(s);    // 测试点4，没有运算符只有一个数字
            else if(!Character.isDigit(ch[0])){                 // 为拼音或者开方
                if(ch[1] == 'q')    cur = (int) Math.sqrt(Integer.parseInt(s.substring(4)));
                else    cur = map.get(s);
            }else{                                              // 有运算符
                int j = 0;
                for (j = 0; j < ch.length; j++) {               // 找到运算符位置（运算的数字可以大于9）
                    if(!Character.isDigit(ch[j]))   break;
                }
                char operator = ch[j];
                int num1 = Integer.parseInt(s.substring(0,j));
                int num2 = Integer.parseInt(s.substring(j+1));
                if(operator == '+')    cur = num1 + num2;
                else if(operator == '-')    cur = num1 - num2;
                else if(operator == '*')    cur = num1 * num2;
                else if(operator == '/')    cur = num1 / num2;
                else if(operator == '%')    cur = num1 % num2;
                else    cur = (int) Math.pow(num1,num2);
            }
            res += cur;
        }
        out.println(res);
        out.flush();
        out.close();
    }
}
```



### 1119.**胖达与盆盆奶**(贪心双向扫描)

```java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        // 读入数据
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine().trim());
        int[] weight = new int[n];
        int[] left = new int[n];  // 从左向右扫描得到的当前熊猫最小奶量
        int[] right = new int[n]; // 从右向左扫描得到的当前熊猫最小奶量
        
        String[] parts = br.readLine().split("\\s+");
        for (int i = 0; i < n; i++) {
            weight[i] = Integer.parseInt(parts[i]);
        }
        
        // 初始化所有奶量至少200毫升
        Arrays.fill(left, 200);
        Arrays.fill(right, 200);
        
        // 从左向右扫描：如果当前熊猫比左边的重，则至少比左边多100；如果相等则相同，否则保持不变
        for (int i = 1; i < n; i++) {
            if (weight[i] > weight[i - 1]) {
                left[i] = left[i - 1] + 100;
            }
            if (weight[i] == weight[i - 1]) {
                left[i] = left[i - 1];
            }
        }
        
        // 从右向左扫描：如果当前熊猫比右边的重，则至少比右边多100；如果相等则相同，否则保持不变
        for (int i = n - 2; i >= 0; i--) {
            if (weight[i] > weight[i + 1]) {
                right[i] = right[i + 1] + 100;
            } 
            if (weight[i] == weight[i + 1]) {
                right[i] = right[i + 1];
            }
        }
        
        // 对于每只熊猫，取两趟扫描得到的奶量中的较大值，即为满足左右条件的最小分配量
        long total = 0;
        for (int i = 0; i < n; i++) {
            total += Math.max(left[i], right[i]);
        }
        
        System.out.println(total);
    }
}

```



### 1120.买地攻略

```java
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    public static void main(String[] args) throws IOException{
        int n = ini();
        int m = ini();
        int[] land = new int[n];
        for(int i = 0;i < n;i++){
            land[i] = ini();
        }
        int count = 0;// 买地方案数
        // 固定起始点，连续购买地至总额超过预算
        for(int i = 0;i < n;i++){
            int sum = 0;
            for(int j = i;j < n;j++){
                sum += land[j];
                if(sum > m){
                    break;
                }else    count++;
            }
        }
        out.println(count);
        out.flush();
        out.close();
    }
}
// 比较极限，超时就多提交几次
```

前缀和方法（会超时）：

```java
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    public static void main(String[] args) throws IOException{
        int n = ini();
        int m = ini();
        int[] pre = new int[n+1];
        for(int i = 1;i <= n;i++){
            pre[i] = pre[i-1] + ini();
        }
        int count = 0;
        for(int i = 1;i <= n;i++){
            for(int j = i;j <= n;j++){
                if(pre[j] - pre[i-1] <= m)    count++;
                else    break;
            }
        }
        out.println(count);
        out.flush();
        out.close();
    }
}
```



### 1121.祖传好运

```java
import java.io.*;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    public static void main(String[] args) throws IOException{
        int k = ini();
        for(int i = 0;i < k;i++){
            int cur = ini();
            String scur = String.valueOf(cur);
            int length = scur.length();     // 当前数的位数
            boolean isLuckyNum = true;      
            while(length > 0){
                if(cur % length != 0){      // 不能整除位数
                    isLuckyNum = false;
                    break;
                }
                cur /= 10;
                length--;
            }
            if(isLuckyNum)    out.println("Yes");
            else    out.println("No");
        }
        out.flush();
        out.close();
    }
}
```



### 1122.找奇葩

```java
import java.io.*;
import java.util.HashMap;

public class Main{
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    public static void main(String[] args) throws IOException{
        int n = ini();
        HashMap<Integer,Integer> map = new HashMap<>(); // 数字和出现次数映射
        for(int i = 0;i < n;i++){
            int cur = ini();
            map.put(cur,map.getOrDefault(cur,0)+1);
        }
        int res = 0;
        for(Integer i : map.keySet()){
            if(map.get(i) %2 != 0 && i % 2 != 0){
                res = i;
                break;
            }
        }
        out.println(res);
        out.flush();
        out.close();
    }
}
```



### 1123.舍入

```java
// 纯模拟，测试点6,7死活过不了是因为整数太大了得用BigInteger存
import java.io.*;
import java.math.BigInteger;
import java.util.Arrays;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);

    static int ini() throws IOException {
        in.nextToken();
        return (int) in.nval;
    }

    public static void main(String[] args) throws IOException {
        int n = ini();
        int d = ini();
        for (int i = 0; i < n; i++) {
            StringBuilder res = new StringBuilder();
            String[] s = ins.readLine().split(" ");
            int command = Integer.parseInt(s[0]);

            // 处理负号
            boolean isNegativeNum = false;
            if (s[1].charAt(0) == '-') {
                s[1] = s[1].substring(1);
                isNegativeNum = true;
            }

            // 提取整数和小数部分
            char[] ch = s[1].toCharArray();
            BigInteger curInt; // 用 BigInteger 代替 long，测试点6,7
            int dotIndex = s[1].indexOf('.');
            if (dotIndex != -1) {
                // 小数点存在，整数部分用 BigInteger 解析
                curInt = new BigInteger(s[1].substring(0, dotIndex));
                // 小数部分前面加上一个'0'，便于后续处理
                String str = "0" + s[1].substring(dotIndex + 1);
                ch = str.toCharArray();
            } else {  // 小数点不存在，测试点2
                curInt = new BigInteger(s[1]);
                ch = new char[2];
                Arrays.fill(ch, '0');
            }

            // 对小数部分进行零扩展，保证长度足够舍入
            char[] decimal = new char[Math.max(d, ch.length) + 2];
            Arrays.fill(decimal, '0');
            for (int j = 0; j < ch.length; j++) {
                decimal[j] = ch[j];
            }

            // 根据舍入规则进行处理
            switch (command) {
                case 1: // 四舍五入（n入5）
                    nAdd(decimal, 5, d);
                    break;
                case 3: // 四舍六入五成双
                    if (decimal[d + 1] == '5') { // 当后一位为5时
                        boolean haveNonZero = false;
                        for (int j = d + 2; j < decimal.length; j++) {
                            if (decimal[j] != '0') {
                                haveNonZero = true;
                                break;
                            }
                        }
                        if (haveNonZero) { // 5 后面有非 0 数，进位
                            nAdd(decimal, 0, d);
                        } else { // 5 后面全为 0
                            if ((decimal[d] - '0') % 2 != 0) { // 若第 d 位为奇数，进位
                                nAdd(decimal, 0, d);
                            }
                        }
                    } else { // 其他情况采用“六入”策略
                        nAdd(decimal, 6, d);
                    }
                    break;
                    // 注意：如果是截断（命令2），不做任何处理
            }

            // 截取需要的小数位
            String resDecimal = new String(decimal).substring(1, d + 1);
            // 如果舍入时小数部分产生了进位，decimal[0]会变为 '1'
            if (decimal[0] == '1') {
                curInt = curInt.add(BigInteger.ONE);
            }
            // 判断小数部分是否全为 0 的逻辑
            boolean isAllZero = true;
            for (int j = 0; j < resDecimal.length(); j++) {
                if (resDecimal.charAt(j) != '0') {
                    isAllZero = false;
                    break;
                }
            }
            // 当原数为负，且结果不为零时添加负号（测试点8要求：0不显示负号）
            if (isNegativeNum && (!isAllZero || curInt.compareTo(BigInteger.ZERO) != 0)) {
                res.append("-");
            }
            res.append(curInt.toString());
            res.append(".");
            res.append(resDecimal);
            out.println(res);
        }
        out.flush();
        out.close();
    }

    // 进行 n 入：从第 d 位小数向前进位
    public static void nAdd(char[] decimal, int n, int d) {
        if (decimal[d + 1] - '0' >= n) {
            int carry = 1;
            for (int j = d; j >= 0; j--) {
                if (carry == 0) break;
                int cur = decimal[j] - '0' + 1;
                if (cur == 10) {
                    decimal[j] = '0';
                } else {
                    carry = 0;
                    decimal[j] = Integer.toString(cur).charAt(0);
                }
            }
        }
    }
}

```

（BigDecimal和RunningMode）版本：（这不是直接调库嘛，真的）

```java
import java.util.Scanner;
import java.math.BigDecimal;
import java.math.RoundingMode;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int N = sc.nextInt();
        int D = sc.nextInt();
        for (int i = 0; i < N; i++) {
            int method = sc.nextInt();
            String numStr = sc.next();
            BigDecimal num = new BigDecimal(numStr);
            BigDecimal result = null;
            switch (method) {
                case 1: // 四舍五入
                    result = num.setScale(D, RoundingMode.HALF_UP);
                    break;
                case 2: // 截断（直接舍去尾数）
                    result = num.setScale(D, RoundingMode.DOWN);
                    break;
                case 3: // 四舍六入五成双
                    result = num.setScale(D, RoundingMode.HALF_EVEN);
                    break;
                default:
                    break;
            }
            // 输出时保留指定位数的小数（自动补0）
            System.out.println(result.toPlainString());
        }
        sc.close();
    }
}
```



### 1124.**最近的斐波那契数**

```java
import java.io.*;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    static StreamTokenizer in = new StreamTokenizer(ins);
    static int ini() throws IOException{
        in.nextToken();
        return (int) in.nval;
    }
    public static void main(String[] args) throws IOException{
        int target = ini();
        int fir = 0;
        int sec = 1;
        while(fir + sec <= target){
            int next = fir + sec;
            fir = sec;
            sec = next;
        }
        int next = fir + sec;
        System.out.println((target-sec) <= (next-target) ? sec : next);
    }
}
```



### 1125.**子串与子列**

```java
import java.io.*;
import java.util.*;

public class Main{
    static BufferedReader ins = new BufferedReader(new InputStreamReader(System.in));
    public static void main(String[] args) throws IOException{
        String s = ins.readLine();
        String p = ins.readLine();
        int index = 0;  // 子列当前比对的位置
        int minLength = s.length();  // 最短长度
        int start = 0;  // 子串的起始位
        int end = 0;    // 子串的终止位
        for(int i = 0;i < s.length();i++){
            if(s.charAt(i) == p.charAt(0)){ // 子列第一个字符匹配则标志子列后续匹配开始
                index = 1;
                for(int j = i+1;j < s.length();j++){
                    if(j-i+1 >= minLength)    break;    // 子列未完全匹配但长度超过已知最短长度时跳出循环
                    if(s.charAt(j) == p.charAt(index))    index++;  // 当前比对位置匹配成功
                    if(index == p.length()){    // 子列全部匹配完则跳出循环
                        start = i;
                        end = j;
                        minLength = j-i+1;
                        break;
                    }
                }
            }
        }
        System.out.println(s.substring(start,end+1));
    }

}
```









