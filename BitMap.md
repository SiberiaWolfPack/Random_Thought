### BitMapJava实现
* 设想这样一个场景，在20亿个随机整数中找到某个数是否在其中，并假设32位操作系统，4G内存。如果每个数字用int存储，那就是20亿个int，也就是20亿*4个byte。也就是20亿*4/1024/1024/1024=7.45G，而如果按位存储，20亿个数就是20亿个bit，也就是20亿/8/1024/1024/1024=233MB，高下立判。
优点：
* 运算效率高，不需要比较和移动位置。
* 占用内存少。
缺点：
* 不能有数据重复。
* 当数据密集时才有优势。

Bitmap的主要应用场合：表示连续（或接近连续，即大部分会出现）的关键字序列的状态（状态数/关键字个数  越小越好）。

```java
package DataStructure;

public class BitmapTest {
    public static void main(String[] args) {

        Bitmap bitmap = new Bitmap(100);
        bitmap.add(7);
        bitmap.add(18);
        bitmap.add(3);

        System.out.println(bitmap.contain(18));
        System.out.println(bitmap.contain(3));

        bitmap.clear(7);
        System.out.println(bitmap.contain(7));
    }
}

class Bitmap {

    private byte[] arr;
    private int capacity;

    public Bitmap(int capacity) {
        this.capacity = capacity;

        // 1bit能存储8个数据，那么capacity数据需要多少个bit呢，capacity/8+1,右移3位相当于除以8
        arr = new byte[(capacity >> 3) + 1];
    }

    public void add(int x) { // 插入操作
        int arrIndex = x >> 3; //  num/8得到byte[]的index
        int byteIndex = x & 0x07; //  num%8得到在byte[index]的位置
        arr[arrIndex] |= 1 << byteIndex; // 将1左移position后，那个位置自然就是1，然后和以前的数据做|，这样，那个位置就替换成1了。
    }

    public void clear(int x) {
        int arrIndex = x >> 3;
        int byteIndex = x & 0x07;
        arr[arrIndex] &= ~(1 << byteIndex); //  //将1左移position后，那个位置自然就是1，然后对取反，再与当前值做&，即可清除当前的位置了
    }

    public boolean contain(int x) {
        int arrIndex = x >> 3;
        int byteIndex = x & 0x07;
        return (arr[arrIndex] & (1 << byteIndex)) != 0; // 将1左移position后，那个位置自然就是1，然后和以前的数据做&，判断是否为0即可
    }
}
```

