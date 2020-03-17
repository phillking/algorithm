字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。

示例

输入: s = "abcdefg", k = 2
输出: "cdefgab"

解法一:

最直接的方法就是把字符串的前n位剪切到末尾。

    string reverseLeftWords(string s, int n) {
        return s.substr(n)+ s.substr(0,n);
    }

解法二：

str=AB，BA=。

反转A，反转B，再反转str.

时间复杂厚度O(n), 空间复杂度O(1);

    void reverse(string& s, int left, int right){
        while(left<right){
            swap(s[left], s[right]);
            left++;
            right--;
        }
    }
    string reverseLeftWords(string s, int n) {
        if(s.empty()){
            return s;
        }
        int len = s.length();
        n = n%len;
        reverse(s, 0, n-1);
        reverse(s, n, len-1);
        reverse(s, 0, len-1);
        return s;

    }
解法三:

循环拷贝，时间复杂度O(n)空间复杂度O(1)。

考察字符串中每个字符转换前后的位置，可以发现两者是存在映射关系的。



输入: s = "abcdefg", k = 2

输出: cdefgba



我们把一整条链按先后顺序排列:a-c-e-g-b-d-f-a, 可以看到这是一个循环列， 其中后一个字符转换到了前一个字符所在的位置，如下图所示:





经过观察，我们可以看到(循环链长度)*(左移数)的长度是二者的最小公倍数；

如果二者不互质，循环链可能有多条，数目为二者的最大公约数，每条循环链的长度为(字符串长度)/(循环链条数)。

例子:

输入 s= abcdef   k=3

输出:   defabc.



代码如下:

    string reverseLeftWords(string s, int n) {
        if(s.empty()){
            return s;
        }
        int len = s.length();
        n = n%len;
        int m = __gcd(len, n);   // number of chain
        int l = len/m;           // length of chain

        for(int i=0; i<m; i++){
            for(int j=0, pre = i; j<l-1; j++,pre+=n){
                swap(s[pre%len], s[(pre+n) %len]);
            }
        }
        return s;

    }
