## 容器



> 数组就是一种容器，可以**在其中放置对象或基本数据类型**


#### 泛型
泛型的本质是“数据类型的参数化”，在调用泛型时必须传入实际类型


#### Collection
        Collection<String> collection = new ArrayList<>();
        collection.add("我");
        collection.add("爱");
        collection.add("你");
        System.out.println(collection);
        System.out.println("集合长度:" + collection.size());
        System.out.println("集合是否为空:" + collection.isEmpty());
        System.out.println("集合是否包含某元素:" + collection.contains("爱"));

        Object[] objects = collection.toArray();
        System.out.println(Arrays.toString(objects));    //转换成一个Object数组


        collection.remove("你");         //移除某个元素
        System.out.println(collection);
        collection.clear();                 //移除所有元素
        System.out.println("集合长度:" + collection.size());


        List<String> listA=new ArrayList<>();
        listA.add("aa");
        listA.add("bb");
        listA.add("cc");

        List<String> listB=new ArrayList<>();
        listB.add("aa");
        listB.add("dd");
        listB.add("ee");

        System.out.println("listA:"+listA);
        System.out.println("listB:"+listB);

        listA.addAll(listB);
        System.out.println("将B添加到A中:"+listA);

        listA.retainAll(listB);
        System.out.println("移除非交集:"+listA);
        
        listA.removeAll(listB);
        System.out.println("移除交集:"+listA);



##### List
List是有序、可重复的容器  
  
        List<String> listA = new ArrayList<>();
        listA.add("aa");
        listA.add("bb");
        listA.add("cc");
        listA.add("dd");
        System.out.println(listA);
        listA.add(2, "我");
        System.out.println("按索引插入：" + listA);
        listA.remove(2);
        System.out.println("按索引移除：" + listA);
        listA.set(2, "你");
        System.out.println("按索引设置值：" + listA);
        listA.add("你");

        System.out.println(listA);
        System.out.println("获取索引值位置的元素：" + listA.get(2));
        System.out.println("获取指定元素第一次出现的索引，未找到返回-1："+listA.indexOf("你"));
        System.out.println("获取指定元素最后出现的索引，未找到返回-1："+listA.lastIndexOf("你"));


List常用的实现类有3个：  
###### 1. ArrayList
ArrayList使用数组实现，查询效率高，增删效率低，线程不安全  


> ArrayList实现

	/**
	 * @program: JavaBasic
	 * @description:
	 * @author: Zhang Shangfeng
	 * @create: 2020-07-16 16:04
	 **/
	public class MyArrayList<T> {
    private final static int DEFAULT_CAPACITY = 10;
    private Object[] elementDate;
    private int size;

    public MyArrayList() {
        elementDate = new Object[DEFAULT_CAPACITY];
    }

    public MyArrayList(int capacity) {
        if (capacity < 0) {
            throw new IndexOutOfBoundsException();
        } else if (capacity == 0) {
            elementDate = new Object[DEFAULT_CAPACITY];
        } else {
            elementDate = new Object[capacity];
        }
    }

    public void add(T object) {
        if (size == elementDate.length) {
            Object[] newArray = new Object[elementDate.length + (elementDate.length >> 1)];
            System.arraycopy(elementDate, 0, newArray, 0, elementDate.length);
            elementDate = newArray;
        }
        elementDate[size++] = object;
    }

    @Override
    public String toString() {
        StringBuilder stringBuilder = new StringBuilder();
        stringBuilder.append("[");
        for (int i = 0; i < size; i++) {
            stringBuilder.append(elementDate[i]).append(",");
        }
        stringBuilder.setCharAt(stringBuilder.length() - 1, ']');
        return stringBuilder.toString();
    }


    public T get(int index) {
        checkRange(index);
        return (T) elementDate[index];
    }

    private void checkRange(int index) {
        if (index < 0 || index > size) {
            throw new IndexOutOfBoundsException();
        }
    }

    public void set(int index, T element) {
        checkRange(index);
        elementDate[index] = element;
    }


    public T remove(int index) {
        checkRange(index);
        T oldValue = get(index);
        int numMoved = elementDate.length - index - 1;
        if (numMoved > 0) {
            System.arraycopy(elementDate, index + 1, elementDate, index, numMoved);
        }
        elementDate[size--] = null;
        return oldValue;
    }

    public boolean remove(T element) {
        if (element == null) {
            for (int i = 0; i < size; i++) {
                if (get(i) == null) {
                    remove(i);
                    return true;
                }
            }
        } else {
            for (int i = 0; i < size; i++) {
                if (get(i).equals(element)) {
                    remove(i);
                    return true;
                }
            }
        }
        return false;
    }
	}




###### 2. LinkedList
LinkedList使用双向链表实现，增删效率高，查找效率低，线程不安全  

	public class MyLinkedList<T> {
    static class Node<T> {
        Node previous;
        Node next;
        T element;

        public Node() {
        }

        public Node(T element) {
            this.element = element;
        }

        public Node(Node previous, Node next, T element) {
            this.previous = previous;
            this.next = next;
            this.element = element;
        }

        public Node getPrevious() {
            return previous;
        }

        public void setPrevious(Node previous) {
            this.previous = previous;
        }

        public Node getNext() {
            return next;
        }

        public void setNext(Node next) {
            this.next = next;
        }

        public T getElement() {
            return element;
        }

        public void setElement(T element) {
            this.element = element;
        }
    }

    private Node first;
    private Node last;
    private int size;


    public void add(T object) {
        Node node = new Node(object);
        if (size == 0) {
            first = node;
            last = node;
            size++;
        } else {
            node.previous = last;
            node.next = first;
            first.previous = node;
            last.next = node;
            last = node;
            size++;
        }
    }

    public Node get(int index) {
        checkRange(index);
        Node temp = first;
        if (index <= (size >> 1)) {
            for (int i = 0; i < index; i++) {
                temp = temp.next;
            }
        } else {
            for (int i = size; i > index; i--) {
                temp = temp.previous;
            }
        }
        return temp;
    }

    private void checkRange(int index) {
        if (index < 0 || index > size - 1) {
            throw new IndexOutOfBoundsException();
        }
    }


    public String toString() {
        Node temp = first;
        StringBuilder stringBuilder = new StringBuilder("[");
        for (int i = 0; i < size; i++) {
            stringBuilder.append(temp.element).append(',');
            temp = temp.next;
        }
        stringBuilder.setCharAt(stringBuilder.length() - 1, ']');
        return String.valueOf(stringBuilder);
    }

    public void remove(int index) {
        Node temp = get(index);
        Node previous = temp.previous;
        Node next = temp.next;
        previous.next = temp.next;
        System.out.println("修改后的previous：" + previous.element);
        System.out.println("修改后的previous的next指向：" + previous.next.element);


        next.previous = temp.previous;
        System.out.println("修改后的next：" + next.element);
        System.out.println("修改后的next的previous指向：" + next.previous.element);

        if (index == 0) {
            first = next;
        }
        size--;
    }

    public void add(int index, T element) {
        checkRange(index);
        Node newNode = new Node(element);
        Node temp = get(index);
        Node previous = temp.previous;

        previous.next = newNode;
        newNode.previous = previous;

        newNode.next = temp;
        temp.previous = newNode;

        size++;

    }

    public static void main(String[] args) {
        MyLinkedList<String> myLinkedList = new MyLinkedList();
        myLinkedList.add("a");
        myLinkedList.add("b");
        myLinkedList.add("c");
        myLinkedList.add("d");
        myLinkedList.add("e");
        myLinkedList.add("f");


        System.out.println(myLinkedList.get(0).element);
        myLinkedList.remove(2);

        myLinkedList.add(2, "newNode");

        System.out.println(myLinkedList.toString());
    }
}
###### 3. Vector
线程安全，效率低  
代码实现和ArrayList类似

##### Map
Map使用键值对来存储，Map中的键不能重复  
Map的实现类有HashMap、TreeMap、HashTable、Properties等  
###### 1、HashMap
HashMap底层实现使用了哈希表（数组+链表）  


##### Set
无序、不可重复


	
##### 迭代器

		List<String> list = new ArrayList<>();
        list.add("a");
        list.add("b");
        list.add("c");

        for (Iterator<String> iterator = list.iterator(); iterator.hasNext(); ) {
            String temp = iterator.next();
            System.out.println(temp);
        }

        Set<String> set = new HashSet<>();
        set.add("a");
        set.add("b");
        set.add("c");
        for (Iterator<String> iterator = set.iterator(); iterator.hasNext(); ) {
            String temp1 = iterator.next();
            System.out.println(temp1);
        }


        Map<Integer, String> map = new HashMap<>();
        map.put(1, "a");
        map.put(2, "b");
        map.put(3, "c");

        Set<Map.Entry<Integer, String>> se = map.entrySet();

        for (Iterator<Map.Entry<Integer, String>> iterator = se.iterator(); iterator.hasNext(); ) {
            Map.Entry<Integer, String> temp3 = iterator.next();
            System.out.println(temp3.getKey() + "---" + temp3.getValue());
        }

        Set<Integer> si = map.keySet();
        for (Iterator<Integer> integerIterator = si.iterator(); integerIterator.hasNext(); ) {
            Integer key = integerIterator.next();
            System.out.println(key + "-----" + map.get(key));
        }

