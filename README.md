# Android NotePad笔记本应用
## 页面展示：
#### 1.主页（P1）:  
![image](https://github.com/Jianlingxi/Exp_Midterm/blob/master/DRAW/1.png?raw=true)  
#### 2.添加Note(P2):  
![image](https://github.com/Jianlingxi/Exp_Midterm/blob/master/DRAW/2.png?raw=true)  
#### 3.侧边菜单栏(P3):  
![image](https://github.com/Jianlingxi/Exp_Midterm/blob/master/DRAW/3.png?raw=true)  
#### 4.设置(P4):  
![image](https://github.com/Jianlingxi/Exp_Midterm/blob/master/DRAW/4.png?raw=true)  
## ①基础功能：

### 1. NoteList中显示条目增加时间戳显示

- #### 思路

  ①在添加笔记的页面中设置TextView以显示取到的时间，并转换成"yyyy-MM-dd HH:mm:ss"

  ②并且在编辑类中新增时间获取方法，当判断在编辑文本后note发生改变进行数据更新，同时时间也做出改变。

- #### 部分实验代码：

  note_layout.xml

  ```xml
  <TextView
          android:id="@+id/tv_time"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:text="Time"
          android:textSize="16dp"
          android:textColor="@color/greyC"/>
  ```

  EditActivity.java

  ```java
  public String dateToStr(){
          Date date = new Date();
          SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
          return simpleDateFormat.format(date);
      }
  ```
  #### 实验截图：
    新增笔记(P5):  
    ![image](https://github.com/Jianlingxi/Exp_Midterm/blob/master/DRAW/5.png?raw=true)  
    更改笔记内容后(P6):  
    ![image](https://github.com/Jianlingxi/Exp_Midterm/blob/master/DRAW/6.png?raw=true)  
### 2.添加笔记查询功能（根据标题查询）

- #### 思路：

  ①主页上增加搜索图标点击后触发输入关键词，接收关键词信息

  ②定义关键词过滤规则存储过滤后的笔记信息并刷新界面

- #### 部分实验代码：

  main_menu.xml

  ```xml
  <item
          android:id="@+id/action_search"
          android:icon="?attr/menu_search"
          app:showAsAction="always"
          app:actionViewClass="androidx.appcompat.widget.SearchView"
          android:title="Search"
          />
  ```

  NoteAdapter.java

  ```java
  class MyFilter extends Filter {
          //定义过滤规则
          @Override
          protected FilterResults performFiltering(CharSequence charSequence) {
              FilterResults result = new FilterResults();
              List<Note> list;
              if (TextUtils.isEmpty(charSequence)) {//显示所有的数据
                  list = backList;
              } else {//符合条件的数据对象添加到集合中
                  list = new ArrayList<>();
                  for (Note note : backList) {
                      if (note.getContent().contains(charSequence)) {
                          list.add(note);
                      }
  
                  }
              }
              result.values = list; //保存集合数据
              result.count = list.size();//保存集合的大小
              return result;
          }
          //适配器更新界面
          @Override
          protected void publishResults(CharSequence charSequence, FilterResults filterResults) {
              noteList = (List<Note>)filterResults.values;
              if (filterResults.count>0){
                  notifyDataSetChanged();//通知数据发生了改变
              }else {
                  notifyDataSetInvalidated();//通知数据失效
              }
          }
      }
  ```

  #### 实验截图：
    原始数据(P7):  
    ![image](https://github.com/Jianlingxi/Exp_Midterm/blob/master/DRAW/7.png?raw=true)
    点击右上角搜索图标(P8):  
    ![image](https://github.com/Jianlingxi/Exp_Midterm/blob/master/DRAW/8.png?raw=true)
    根据搜索框内容进行筛选(P9):  
    ![image](https://github.com/Jianlingxi/Exp_Midterm/blob/master/DRAW/9.png?raw=true)

## ②附加功能：

### 1. UI美化、偏好设置、白天深夜模式切换

* #### 思路

  ①预先编写主题样式

  ②当切换深夜模式对主题样式进行修改

* #### 部分实验代码：

  MainActivity.java

  ```java
  //黑夜模式
  private void setSelfNightMode(){
          //重新赋值，重启Activity
          super.setNightMode();
          Intent intent = new Intent(this, UserSettingsActivity.class);
          intent.putExtra("night_change", !night_change); //正负颠倒。
          startActivity(new Intent(this, UserSettingsActivity.class));
          overridePendingTransition(R.anim.night_switch, R.anim.night_switch_over);
          finish();
      }
      private void setNightModePref(boolean night){
          //通过nightMode switch修改pref中的nightMode
          sharedPreferences = PreferenceManager.getDefaultSharedPreferences(getBaseContext());
          SharedPreferences.Editor editor = sharedPreferences.edit();
          editor.putBoolean("nightMode", night);
          editor.commit();
      }
  ```

  ```java
    //根据 preference.xml中的fabColor值调整fab颜色  主页菜单栏的颜色
    private void chooseFabColor(int fabColor) {

        switch (fabColor) {
            case -500072:
                fab.setBackgroundTintList(ColorStateList.valueOf(getColor(R.color.q)));
                myToolbar.setBackgroundTintList(ColorStateList.valueOf(getColor(R.color.q)));
                break;
            case -500081:
                fab.setBackgroundTintList(ColorStateList.valueOf(getColor(R.color.w)));
                myToolbar.setBackgroundTintList(ColorStateList.valueOf(getColor(R.color.w)));
                break;
            case -500061:
                fab.setBackgroundTintList(ColorStateList.valueOf(getColor(R.color.e)));
                myToolbar.setBackgroundTintList(ColorStateList.valueOf(getColor(R.color.e)));
                break;
            case -500074:
                fab.setBackgroundTintList(ColorStateList.valueOf(getColor(R.color.r)));
                myToolbar.setBackgroundTintList(ColorStateList.valueOf(getColor(R.color.r)));
                break;
            case ...
    }
    ```
  
  #### 实验截图：
  
    深夜模式(P10 P11):  
    ![image](https://github.com/Jianlingxi/Exp_Midterm/blob/master/DRAW/10.png?raw=true)![image](https://github.com/Jianlingxi/Exp_Midterm/blob/master/DRAW/11.png?raw=true)  
  
    白天模式(P12 P13):  
    ![image](https://github.com/Jianlingxi/Exp_Midterm/blob/master/DRAW/12.png?raw=true)![image](https://github.com/Jianlingxi/Exp_Midterm/blob/master/DRAW/13.png?raw=true)  
 
  
    ##### 修改添加钮和工具栏的颜色样式: 
    
    修改前(P14):  
    ![image](https://github.com/Jianlingxi/Exp_Midterm/blob/master/DRAW/14.png?raw=true)  
    修改后的效果(P15):  
    ![image](https://github.com/Jianlingxi/Exp_Midterm/blob/master/DRAW/15.png?raw=true)  


### 2.  按时间正序或倒序排列笔记

* #### 思路

  ①编写倒置等相关方法对数据库中已有的Note项进行相应操作


* #### 部分实验代码：

  MainActivity.java

  ```java
  //按模式时间排序笔记
      public void sortNotes(List<Note> noteList, final int mode) {
          Collections.sort(noteList, new Comparator<Note>() {
              @Override
              public int compare(Note o1, Note o2) {
                  try {
                      if (mode == 1) {
                          Log.d(TAG, "sortnotes 1");
                          return npLong(dateStrToSec(o2.getTime()) - dateStrToSec(o1.getTime()));
                      }
                      else if (mode == 2) {//倒置
                          Log.d(TAG, "sortnotes 2");
                          return npLong(dateStrToSec(o1.getTime()) - dateStrToSec(o2.getTime()));
                      }
  //按模式时间排序计划
 
  ```


  #### 实验截图：
  
开启按时间倒序排列(P16):                  
 ![image](https://github.com/Jianlingxi/Exp_Midterm/blob/master/DRAW/16.png?raw=true)  
倒序排列后的效果(P17):  
  ![image](https://github.com/Jianlingxi/Exp_Midterm/blob/master/DRAW/17.png?raw=true)

  


### 3. 笔记标签分类，可自定义新标签，按标签分类统计/查询

* #### 思路

  ①新增数据项属性标签，可自定义标签名称

  ②若监听到触发根据标签分类查询的方法按该标签对象对已有数据项进行筛选刷新展示

* #### 部分实验代码：

  MainAcitivty.java

  ```java
  //新增自定义标签
  add_tag.setOnClickListener(new OnClickListener() {
                      @Override
                      public void onClick(View v) {
                          if (sharedPreferences.getString("tagListString","").split("_").length < 8) {
                              final EditText et = new EditText(context);
                              new AlertDialog.Builder(MainActivity.this)
                                      .setMessage("Enter the name of tag")
                                      .setView(et)
                                  ......
  }
          else Toast.makeText(context, "Repeated tag!", Toast.LENGTH_SHORT).show();
  	}                           ......
  //按选中的标签进行展示
  List<String> tagList = Arrays.asList(sharedPreferences.getString("tagListString", null).split("_")); //获取tags
                  tagAdapter = new TagAdapter(context, tagList, numOfTagNotes(tagList));
                  lv_tag.setAdapter(tagAdapter);
  
                  lv_tag.setOnItemClickListener(new OnItemClickListener() {
                      @Override
                      public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                          List<String> tagList = Arrays.asList
                              	......
  ```

  #### 实验截图：
  
添加前(P18):  
![image](https://github.com/Jianlingxi/Exp_Midterm/blob/master/DRAW/18.png?raw=true)  
点击add new tag 输入自定义tag名称(P19)：  
![image](https://github.com/Jianlingxi/Exp_Midterm/blob/master/DRAW/19.png?raw=true)  
点击相应标签项只显示该标签下的Note(P20)：  
![image](https://github.com/Jianlingxi/Exp_Midterm/blob/master/DRAW/20.png?raw=true)
  




