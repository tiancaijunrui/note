---
title: java-utils
date: 2018-04-10 15:21:08
tags: java
---

### java 常用工具类

- 获取Date的yyyy-MM-dd 23:59:59

  ```
    public static Date getEndTimeOfDate(Date date) {
          if (date == null) {
              return null;
          }
          Calendar calendar = Calendar.getInstance();
          calendar.setTime(date);
          calendar.set(Calendar.HOUR_OF_DAY, 23);
          calendar.set(Calendar.MINUTE, 59);
          calendar.set(Calendar.SECOND, 59);
          return calendar.getTime();
      }
  ```

- 获取Date的yyyy-MM-dd 00:00:00

  ```
  public static Date getBeginOfDate(Date date) {
          if (date == null) {
              return null;
          }
          Calendar calendar = Calendar.getInstance();
          calendar.setTime(date);
          calendar.set(Calendar.HOUR_OF_DAY, 0);
          calendar.set(Calendar.MINUTE, 0);
          calendar.set(Calendar.SECOND, 0);
          calendar.set(Calendar.MILLISECOND, 0);
          return calendar.getTime();
      }
  ```

- 格式化为yyyy-MM-dd形式的字符串

  ```
  public static String formatDate(Date date) {
          if (date == null) {
              return null;
          }
          SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
          return sdf.format(date);
      }
  ```

- 格式化为yyyy-MM-dd hh:mm:ss形式的字符串

  ```
   public static String formatTime(Date date) {
          if (date == null) {
              return null;
          }
          SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
          return sdf.format(date);
      }
  ```

- 将字符串类型的List转化为以","分割的字符串

  ```
  public static String listToString(List<String> list) {
          if (CollectionUtils.isEmpty(list)) {
              throw new ElsException(ElsErrCode.ILLEGAL_PARAM, "List must be not empty!");
          }

          StringBuilder builder = new StringBuilder();
          for (String str : list) {
              builder.append(str).append(",");
          }

          String toString = builder.toString();
          return toString.substring(0, toString.length() - 1);
      }
  ```

- 将以","分割的字符串转化为List<String>,每个元素左右去空操作

  ```
  public static List<String> stringToListTrim(String str) {
          if (StringUtils.isBlank(str)) {
              throw new ElsException(ElsErrCode.ILLEGAL_PARAM, "Str must be not blank!");
          }

          String[] array = str.split(",");
          List<String> list = new ArrayList<String>(array.length);
          for (int i = 0; i < array.length; i++) {
              list.add(array[i].trim());
          }
          return list;
      }
  ```

- list去重

  ```
  public static <E> List<E> difference(Collection<E> source, Collection<E> remove) {
          if (CollectionUtils.isEmpty(source)) {
              return new ArrayList<E>(0);
          }
          if (CollectionUtils.isEmpty(remove)) {
              return new ArrayList<E>(source);
          }
          Map<E, Boolean> elementMap = new HashMap<E, Boolean>(remove.size());
          for (E element : remove) {
              elementMap.put(element, true);
          }

          List<E> result = new ArrayList<E>(source);
          Iterator<E> iterator = result.iterator();
          while (iterator.hasNext()) {
              E next = iterator.next();
              if (BooleanUtils.isTrue(elementMap.get(next))) {
                  iterator.remove();
              }
          }
          return result;
      }
  ```

- 取两个集合的交集

  ```
  public static <E> List<E> intersection(Collection<E> source, Collection<E> remove) {
          if (CollectionUtils.isEmpty(source)) {
              return new ArrayList<E>(0);
          }

          if (CollectionUtils.isEmpty(remove)) {
              return new ArrayList<E>(0);
          }

          Map<E, Integer> elementMap = new HashMap<E, Integer>(remove.size());
          for (E element : remove) {
              Integer cnt = elementMap.get(element);
              if (cnt == null) {
                  cnt = 0;
              }

              elementMap.put(element, ++cnt);
          }

          List<E> result = new ArrayList<E>(source);
          Iterator<E> iterator = result.iterator();
          while (iterator.hasNext()) {
              E next = iterator.next();
              Integer cnt = elementMap.get(next);
              if (cnt != null && cnt != 0) {
                  elementMap.put(next, --cnt);
              } else {
                  iterator.remove();
              }
          }
          return result;
      }
  ```

-  取两个集合的并集，如果有一个集合为空，返回另一个集合

  ```
  public static <E> List<E> union(List<E> source1, List<E> source2) {
          if (CollectionUtils.isEmpty(source1) || CollectionUtils.isEmpty(source2)) {
              if (CollectionUtils.isNotEmpty(source1)) {
                  return source1;
              }

              if (CollectionUtils.isNotEmpty(source2)) {
                  return source2;
              }
              return new ArrayList<E>(0);
          }

          return (List<E>) CollectionUtils.union(source1, source2);
      }
  ```

- 根据限制长度截取字符串（字符串中包括汉字，一个汉字等于两个字符）

  ```
  public static String splitStrByLength(String strParameter, int limitLength) {
  		String return_str = strParameter;//返回的字符串
  		int temp_int = 0;//将汉字转换成两个字符后的字符串长度
  		int cut_int = 0;//对原始字符串截取的长度
  		byte[] b = strParameter.getBytes();//将字符串转换成字符数组

  		for (int i = 0; i < b.length; i++) {
  			if (b[i] >= 0) {
  				temp_int = temp_int + 1;
  			} else {
  				temp_int = temp_int + 2;//一个汉字等于两个字符
  				i++;
  			}
  			cut_int++;

  			if (temp_int >= limitLength) {
  				if (temp_int % 2 != 0 && b[temp_int - 1] < 0) {
  					cut_int--;
  				}
  				if (cut_int > return_str.length()) {
  					return return_str;
  				}
  				return return_str.substring(0, cut_int) + "...";
  			}
  		}
  		return return_str;
  	}
  ```

- json string 转换为 map 对象

  ```
   public static Map<Object, Object> jsonToMap(Object jsonObj) {
          JSONObject jsonObject = JSONObject.fromObject(jsonObj);
          Map<Object, Object> map = (Map)jsonObject;
          return map;
      }
  ```

- json string 转换为 对象

  ```
   public  static <T>  T jsonToBean(Object jsonObj, Class<T> type) {
          JSONObject jsonObject = JSONObject.fromObject(jsonObj);
          T obj =(T)JSONObject.toBean(jsonObject, type);
          return obj;
      }
  ```

- 格式化数字

  ```
  /**
       * 格式化数字
       *
       * @param number    数值
       * @param formatStr 小数点位数 支持的格式有，如果为null 则默认 .##
       *                  <ul>
       *                  <li>0</li>
       *                  <li>0.00</li>
       *                  <li>#</li>
       *                  <li>#.##%</li>
       *                  <li>#.#####E0</li>
       *                  <li>00.####E0</li>
       *                  <li>,###</li>
       *                  <li>,###元每小时。</li>
       *                  </ul>
       * @return 格式化后的数值
       */
      public static double keepDigital(Number number, String formatStr) {
          if (number == null) {
              return 0;
          }

          if (StringUtils.isBlank(formatStr)) {
              formatStr = ".##";
          }
          DecimalFormat decimalFormat = new DecimalFormat(formatStr);
          return Double.valueOf(decimalFormat.format(number));
      }
  ```

- 格式化数字 字符串类型

  ```
    public static String keepDigitalString(Number number, String formatStr) {
          if (number == null) {
              return "--";
          }

          if (StringUtils.isBlank(formatStr)) {
              formatStr = ".##";
          }

          DecimalFormat decimalFormat = new DecimalFormat(formatStr);
          if ("0".equals(number.toString())) {
              return "0" + decimalFormat.format(number);
          }

          return decimalFormat.format(number);
      }
  ```

  ​