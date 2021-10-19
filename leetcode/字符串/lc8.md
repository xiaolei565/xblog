# [8. 字符串转换整数 (atoi)](https://leetcode-cn.com/problems/string-to-integer-atoi/)

```java
class Solution {
    public int myAtoi(String s) {
        //1.去除前导0
        s = s.trim();
        //在这判断一下是否为空
        if(s.length()==0){
            return 0;
        }
        //2.检查下一个字符是否为负号
        int flag = 1;
        int index = 0;
        if(s.charAt(0)=='-'){
            flag = -1;
            index++;
        }else if(s.charAt(0)=='+'){
            index++;
        }
        //3.开始读入数字字符，非数字字符结束
        long res = 0;
        while(index<s.length()){
            if(Character.isDigit(s.charAt(index))){
                res = res*10+(s.charAt(index++)-'0');
            }else{
                break;
            }
            if(flag==1 && res>=Integer.MAX_VALUE){
                return Integer.MAX_VALUE;
            }
            if(flag==-1 && res>Integer.MAX_VALUE){
                return Integer.MIN_VALUE;
            }
        }
        return (int)res*flag;

    }
}
```

