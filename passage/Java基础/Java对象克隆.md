## Java对象的克隆

### Java对象的复制

- 浅拷贝：在某些场景中，我们需要获取到一个对象的拷贝用于某些处理。这时候就可以用到Java中的Object.clone方法进行对象复制，得到一个一模一样的新对象。但是在实际使用过程中会发现：当对象中含有可变的引用类型属性时，在复制得到的新对象对该引用类型属性内容进行修改，原始对象响应的属性内容也会发生变化，这就是"浅拷贝"的现象。

- 浅拷贝测试代码：
Person类
```java
public class Person implements Cloneable {
    private int age;
    private String name;
    private Address address;

    public Person(int age, String name, Address address) {
        this.age = age;
        this.name = name;
        this.address = address;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }

    public String display() {
        return "Person{" +
                "age=" + age +
                ", name='" + name + '\'' +
                ", address=" + address +
                '}';
    }
}
```
Address类：
```java
public class Address {
    private String province;
    private String street;

    public Address(String province, String street) {
        this.province = province;
        this.street = street;
    }

    public String getProvince() {
        return province;
    }

    public void setProvince(String province) {
        this.province = province;
    }

    public String getStreet() {
        return street;
    }

    public void setStreet(String street) {
        this.street = street;
    }

    @Override
    public String toString() {
        return "Address [province=" + province + ", street=" + street + "]";
    }
}
```
测试类：
```java
public class Test {
    public static void main(String[] args) throws CloneNotSupportedException {
            Person person = new Person(23, "zhangsan", new Address("上海", "曹安公路4800"));
    
            Person clonePerson = (Person) person.clone();
    
            int i = 1;
    
            System.out.println(i++ +" "+person);
            System.out.println(i++ +" "+clonePerson);
    
            System.out.println(i++ +" "+person.display());
            System.out.println(i++ +" "+clonePerson.display());
    
            clonePerson.setName("lisi");
            clonePerson.setAge(20);
            Address address = clonePerson.getAddress();
            address.setStreet("四平路");
            System.out.println(i++ +" "+clonePerson.display());
            System.out.println(i++ +" "+person.display());
    
    }
}
```
测试结果
```text
1 knowledge.clone.Person@27c170f0
2 knowledge.clone.Person@5451c3a8
3 Person{age=23, name='zhangsan', address=Address [province=上海, street=曹安公路4800]}
4 Person{age=23, name='zhangsan', address=Address [province=上海, street=曹安公路4800]}
5 Person{age=20, name='lisi', address=Address [province=上海, street=四平路]}
6 Person{age=23, name='zhangsan', address=Address [province=上海, street=四平路]}
```

第1、2句输出说明了原对象与新对象是两个不同的对象。

第3、4句可以看到拷贝出来的新对象与原对象内容一致

但是，接着将新对象里面的信息进行了修改，然后输出发现原对象里面的部分信息也跟着变了。仔细观察发现原对象跟着变化的只是Address部分，这就跟clone本身的浅拷贝有关系了。

浅拷贝：创建一个新对象，然后将当前对象的非静态字段复制到该对象，如果字段类型是值类型（基本类型）的，那么对该字段进行复制；如果字段是引用类型的，则只复制该字段的引用而不复制引用指向的对象。此时新对象里面的引用类型字段相当于是原始对象里面引用类型字段的一个副本，原始对象与新对象里面的引用字段指向的是同一个对象。

因此，修改clonePerson里面的address内容时，原person里面的address内容会跟着改变。