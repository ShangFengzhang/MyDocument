## String

> String类又称作不可变字符序列

<h3> 常用方法</h3>
    charAt(i);	//提取下标为i的字符
	length();	//返回长度
	equals();	//比较字符串是否相等
	equalsIgnoreCase()	//忽略大小写比较字符串
	indexOf();	//字符串中是否包含
	replace();	//替换
	startsWith();	//是否以某字符开始
	endWith();		//是否以某字符结尾
	substring();	//从某下标开始截取字符串，截取到第二个参数前
	toLowerCase();	//全转换小写
	toUpperCase();	//全转换大写
	trim();	//去除首位空格


StringBuilder和StringBuffer都继承自AbstractStringBuilder  
StringBuilder是可变的字符序列，线程不安全，效率高
StringBuffer线程安全，效率低

        StringBuilder stringBuilder = new StringBuilder();
        for (int i = 0; i < 5; i++) {
            stringBuilder.append(i);
        }
        System.out.println(stringBuilder);
        stringBuilder.reverse();    //倒序
        System.out.println(stringBuilder);
        stringBuilder.setCharAt(1, '2');
        System.out.println(stringBuilder);
        stringBuilder.insert(0, '我').insert(2, '你');
        System.out.println(stringBuilder);
        stringBuilder.delete(0,2);
        System.out.println(stringBuilder);


StringBuilder和StringBuffer操作原字符串  
**进行累加操作时一定不要使用String进行操作！！！**