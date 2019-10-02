### compare()与compareTo()

- compareTo

compareTo(Object o)方法是java.lang.Comparable<T>接口中的方法，当需要对某个类的对象进行排序时，该类需要实现Comparable<T>接口的，必须重写public int compareTo(T o)方法。
它强行将实现它的每一个类的对象进行整体排序-----称为该类的自然排序，实现此接口的对象列表和数组可以用Collections.sort(),和Arrays.sort()进行自动排序；
也就是说，只要实现了这个接口的对象（数组）就相当于有了排序的能力，所以叫做comparable---可排序的，所以可以说这是一种内部排序的方式，通过实现它唯一的方法compareTo()。
比如Hadoop中的MapReduce，Map函数和Reduce函数处理的 <key,value>,其中需要根据key对键值对进行排序，所以，key实现了WritableComparable<T>接口，实现这个接口可同时用于序列化和反序列化。WritableComparable<T>接口(用于序列化和反序列化)是Writable接口和Comparable<T>接口的组合；

- compare

compare(Object o1,Object o2)方法是java.util.Comparator<T>接口的方法，它实际上用的是待比较对象的compareTo(Object o)方法。
对于它，则是针对一些本身没有比较能力的对象（数组）为它们实现比较的功能，所以它叫做比较器，是一个外部的东西，通过它定义比较的方式，再传到Collection.sort()和Arrays.sort()中对目标排序，而且通过自身的方法compare()定义比较的内容和结果的升降序；


### 案例分析1

统计一个字符串中出现的字母（区分大小写），数字，并按照由大到小输出，如果数字相同按照ASCII码输出
```java
public class Test {

    public static void main(String[] args) {
        Test test = new Test();
        String str = "abbsdd 2339001";
        Map map = test.statistics(str, "small2big");
        test.print(map);
        System.out.println("--------------");
        test.sortPrint(map);
    }

    public Map statistics(String str, String sort) {
        Map<Character, Integer> map = new TreeMap<>();
        char[] chars = str.toCharArray();
        for (char c : chars) {
            if (!('a' <= c && c <= 'z' || 'A' <= c && c <= 'Z' || '0' <= c && c <= '9')) {
                continue;
            }
            if (map.containsKey(c)) {
                map.put(c, map.get(c) + 1);
            } else {
                map.put(c, 1);
            }
        }
        return map;
    }

    public void sortPrint(Map map) {
        /**
         * 通过List进行排序
         */
        List<Entry<Character, Integer>> entries = new ArrayList<Entry<Character, Integer>>(map.entrySet());
        Collections.sort(entries, new Comparator<Entry<Character, Integer>>() {
            @Override
            public int compare(Entry<Character, Integer> o1, Entry<Character, Integer> o2) {
                int result = o1.getValue().compareTo(o2.getValue());
                if (result == 0) {
                    return o1.getKey().compareTo(o2.getKey());
                }
                return result;
            }
        });
        /**
         * 循环输出
         */
        for (Entry<Character, Integer> entry : entries) {
            System.out.println(entry.getKey() + "出现的次数是:" + entry.getValue());
        }
    }

}
```


### 案例分析2

输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。

```java
public class PrintMinNumber {
    public String PrintMinNumber(int [] numbers) {
        StringBuffer sb = new StringBuffer();
        ArrayList<Integer> list = new ArrayList<>();
        int len = numbers.length;
        for(int i=0;i<len;i++){
            list.add(numbers[i]);
        }
        Collections.sort(list, new Comparator<Integer>() {
            @Override
            public int compare(Integer str1, Integer str2) {
                String s1 = ""+str1+str2;
                String s2 = ""+str2+str1;
                return s1.compareTo(s2);
            }
        });
        for(int j : list){
            sb.append(j);
        }
        return sb.toString();
    }
}
```

TreeMap默认对key进行排序，如果对Value进行排序可以放入List中然后重写Comparator

```java
        Map<Character, Integer> map = null;
        switch (sort) {
            case "big2small":
                map = new TreeMap<>(new Comparator<Character>() {
                    @Override
                    public int compare(Character o1, Character o2) {
                        return o1.compareTo(o2);
                    }
                });
                break;
            case "small2big":
                map = new TreeMap<>(new Comparator<Character>() {
                    @Override
                    public int compare(Character o1, Character o2) {
                        return o2.compareTo(o1);
                    }
                });
                break;
            default:
                map = new TreeMap<>();
        }
```
