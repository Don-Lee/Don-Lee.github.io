---
layout:     post
title:      AppCompatSpinner 样式设置
subtitle:   
date:       2018-03-01
author:     Don
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - Android
---
本文讲解内容包含以下几个内容：  
	1. AppCompatSpinner字体大小及颜色设置    
	2. 分割线添加    
	3. 选中item时颜色变化动画      
效果图如下：    
<img src="/img/article/spinner.gif" widht="300px" height="550px"/>

实现以上效果，分3步：  
#### 1.下拉列表框效果实现  
将以下代码复制到styles.xml中即可
```java
<style name="spinner" parent="Widget.AppCompat.DropDownItem.Spinner">
	<!--设置分割线-->
        <item name="android:dropDownListViewStyle">@style/spinnerListStyle</item>
	<!--设置字体颜色及大小-->
        <item name="android:textColor">@color/pm25_good</item>
        <item name="android:textSize">16sp</item>
    </style>

    <style name="spinnerListStyle" parent="@android:style/Widget.ListView.DropDown">
        <item name="android:divider">@color/default_color</item>
        <item name="android:dividerHeight">1dp</item>

        <!-- item按下效果 -->
        <item name="android:listSelector">@android:color/holo_red_light</item>
    </style>
```

#### 2.AppCompatSpinner显示内容的TextView样式设置

我的android架构为MVVM，所以下面的代码看起来有点怪，如果您的代码非MVVM架构，直接使用方法内的代码即可    
DataBindingUtils.java类
```java
    @BindingAdapter("setTextColor")
    public static void setTextColor(AppCompatSpinner spinner, @ColorInt int color) {
        spinner.getViewTreeObserver().addOnGlobalLayoutListener(() -> ((TextView) spinner.getSelectedView()).setTextColor(color));
    }
```


#### 3.xml代码如下：
```java
<android.support.v7.widget.AppCompatSpinner
                    android:id="@+id/spinner_decorate_status"
                    android:layout_width="0dp"
                    android:layout_height="@dimen/height_48_default"
                    android:paddingEnd="@dimen/spacing_16"
                    app:permission="@{permission}"
                    android:adapter="@{decorateAdapter}"
                    android:entries="@{decorateStatusList}"
                    android:selectedItemPosition="@={model.renovationStatusIndex}"
                    android:background="@android:color/white"
                    setTextColor="@{@color/default_color}" /*此句代码设置颜色，调用DataBindingUtils.java类中方法*/
                    android:theme="@style/spinner"	/*此句代码调用步骤1中的样式设置下拉框样式*/
                    android:overlapAnchor="false"  /*此句代码设置下拉框显示在控件下方*/
                    app:layout_constraintStart_toEndOf="@id/tv_decorate_status"
                    app:layout_constraintTop_toTopOf="@id/tv_decorate_status"
                    app:layout_constraintBottom_toBottomOf="@id/tv_decorate_status"
                    app:layout_constraintEnd_toEndOf="parent"/>
```


