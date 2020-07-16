## File


        File f1 = new File("d:/a.txt");
        File f2 = new File("d:\\a.txt");
        f1.renameTo(new File("d:/b.text"));
        System.out.println(System.getProperty("user,dir"));     //获取项目目录
        File f3 = new File("zsf.txt");
        f3.createNewFile();

        System.out.println("File是否存在：" + f3.exists());
        System.out.println("File是否是目录：" + f3.isDirectory());
        System.out.println("File是否是文件：" + f3.isFile());
        System.out.println("File最后修改时间：" + new Date(f3.lastModified()));
        System.out.println("File的大小：" + f3.length());
        System.out.println("File的名字：" + f3.getName());
        System.out.println("File的路径：" + f3.getAbsolutePath());

		//创建目录
        boolean b = f3.mkdir();
        boolean b1 = f3.mkdirs();
