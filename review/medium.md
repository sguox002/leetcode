
12. Integer to Roman
it is not easy to use all the rules, but if we write all thousands, hundreds, tens and ones for all combination, this is really simple
sometimes, if the cases are not much, direct approach could be much simpler
```cpp
string intToRoman(int num) {
    string table[4][10] = {{"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"},
                           {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"},
                           {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"},
                           {"", "M", "MM", "MMM"}
                          };
    string result;
    int count = 0;
    while(num > 0){
        int temp = num % 10;
        result = table[count][temp] + result;
        num /= 10;
        count++;
    }
    return result;
}
````

33. Search in Rotated Sorted Array
binary search
when we divide into two parts [l,mid], [mid,r] there will always one part is sorted.
if the target falls in the sorted region, it is regular binary search 
if the target falls in the non-sorted region, and reducing to a sub problem
