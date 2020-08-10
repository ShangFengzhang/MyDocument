## LocalDate

LocalDate线程安全

        LocalDate localDate = LocalDate.now();
        System.out.println("当前时间为：" + localDate);

        LocalDate appointDate = LocalDate.of(2020, 1, 1);
        System.out.println("获取指定时间：" + appointDate);
        appointDate = LocalDate.of(2020, Month.NOVEMBER, 2);
        System.out.println("获取指定时间：" + appointDate);
        // 严格按照ISO yyyy-MM-dd验证
        LocalDate stringDate = LocalDate.parse("2019-11-03");
        System.out.println("2019年11月3日：" + stringDate);
        // 也可以允许自己定义格式
        stringDate = LocalDate.parse("20191104", DateTimeFormatter.ofPattern("yyyyMMdd"));
        System.out.println("2019年11月4日：" + stringDate);

        LocalDate today = LocalDate.now();
        // 本月第一天
        LocalDate dateOfThisMonth = today.with(TemporalAdjusters.firstDayOfMonth());
        System.out.println("本月第一天：" + dateOfThisMonth);
        // 本月第十天
        dateOfThisMonth = today.withDayOfMonth(10);
        System.out.println("本月第十天：" + dateOfThisMonth);
        // 本月最后一天
        dateOfThisMonth = today.with(TemporalAdjusters.lastDayOfMonth());
        System.out.println("本月最后一天：" + dateOfThisMonth);
        // 下个月的3号（最后一天往后推三天）
        dateOfThisMonth = dateOfThisMonth.plusDays(3);
        System.out.println("下个月3号：" + dateOfThisMonth);
        // 本月第三个周三（先获取第一个周三，然后往后推14天）
        dateOfThisMonth = today.with(TemporalAdjusters.firstInMonth(DayOfWeek.WEDNESDAY)).plusDays(7 * 2);
        System.out.println("本月第三个周三：" + dateOfThisMonth);
        // 下个月的第四个周四（先获取下个月的第1天，再获取第一个周四，然后往后推21天）
        dateOfThisMonth = today.with(TemporalAdjusters.firstDayOfNextMonth()).with(TemporalAdjusters.firstInMonth(DayOfWeek.THURSDAY)).plusDays(7*3);
        System.out.println("下个月的第四个周四：" + dateOfThisMonth);

        // 转字符串
        String todayString = LocalDate.now().toString();
        System.out.println("String-1：" + todayString);
        todayString = LocalDate.now().format(DateTimeFormatter.ofPattern("yyyy/MM/dd"));
        System.out.println("String-2：" + todayString);

        // Local -> Date （这里使用的时间是0点）
        Date todayOfDate = Date.from(LocalDate.now().atStartOfDay(ZoneId.systemDefault()).toInstant());
        System.out.println("todayOfDate：" + todayOfDate);

        // Date -> Local
        LocalDate todayOfLocalDate = new Date().toInstant().atZone(ZoneId.systemDefault()).toLocalDate();
        System.out.println("todayOfLocalDate：" + todayOfLocalDate);


<h3> 日历 </h3>

        Scanner scanner = new Scanner(System.in);
        String str = scanner.next();
        LocalDate date = LocalDate.parse(str,DateTimeFormatter.ofPattern("yyyy-MM-dd"));

        int month=date.getMonthValue();
        System.out.println(month);
        int day=date.getDayOfMonth();
        System.out.println(day);

        date=date.minusDays(day-1);  //产生昨天的日期副本
        DayOfWeek week=date.getDayOfWeek(); //星期几
        System.out.println(week);
        int value=week.getValue();
        System.out.println(value);

        System.out.println("日\t一\t二\t三\t四\t五\t六");


        LocalDate up = date.minusMonths(1); //减去指定月份
        int last = up.with(TemporalAdjusters.lastDayOfMonth()).getDayOfMonth();
        for(int i = value-1; i>0 ; i--){
            System.out.printf("%3d",last-i+1);
            System.out.print(" ");
            //System.out.print(last-i);
        }


        while(date.getMonthValue() == month){
            //格式化输出当前日期在月份中的第几天
            System.out.printf("%3d",date.getDayOfMonth());
            //当前月份日期与，给定的日期相等时，则在后面加上*
            if(date.getDayOfMonth() == day){
                System.out.print("*");
            }else{
                System.out.print(" ");
            }
            //每打印一个日期后，则把当前日期向后移一天
            date = date.plusDays(1);
            //如果当前日期，是一个星期的星期一，则打印一个换行符。
            if(date.getDayOfWeek().getValue() == 1) System.out.println();
        }
        if(date.getDayOfWeek().getValue() != 1) System.out.println();

    }