---
id: 39
title: 안드로이드 커스텀뷰 이해하기
date: 2014-11-05T15:39:07+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=39
permalink: '/%ec%95%88%eb%93%9c%eb%a1%9c%ec%9d%b4%eb%93%9c-%ec%bb%a4%ec%8a%a4%ed%85%80%eb%b7%b0-%ec%9d%b4%ed%95%b4%ed%95%98%ea%b8%b0/'
dsq_thread_id:
  - "3708150521"
categories:
  - Android
tags:
  - android
  - custom view
---
안드로이드 뷰는 화면에 그려지기 전에 아래 그림과 같은 몇 단계의 과정을 거친다. 커스텀뷰를 만들기 위해서는 뷰의 드로잉 과정을 이해해야 한다.

[<img class="size-medium wp-image-41 aligncenter" src="http://i0.wp.com/blog.burt.pe.kr/wp-content/uploads/2014/11/android_view_phrase-300x115.png?resize=300%2C115" alt="" srcset="http://i2.wp.com/burt.pe.kr/wp-content/uploads/2014/11/android_view_phrase.png?resize=300%2C115 300w, http://i2.wp.com/burt.pe.kr/wp-content/uploads/2014/11/android_view_phrase.png?w=447 447w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](http://i0.wp.com/blog.burt.pe.kr/wp-content/uploads/2014/11/android_view_phrase.png)위 단계를 3개의 과정으로 나눌 수 있다. 하나의 과정이 실행되면 항상 Draws 단계에서 마무리 된다.

  1. Animate 과정
  2. Layout 과정
  3. Draw 과정

각 과정의 시작점은 아래와 같다.

  1. Animate 과정은 View의 animate() 메서드에 의해서 시작된다.
  2. Layout 과정은 requestLayout() 메서드에 의해서 시작된다.
  3. Draw 과정은 invalidate() 메서드에 의해서 시작된다.

안드로이드에서 커스텀뷰는 두 종류로 나뉜다.

  1. 자식뷰가 있는 뷰. 즉 뷰그룹
  2. 자식뷰가 없는 뷰.

안드로이드에서 뷰를 화면에 배치시키는 책임은 뷰를 담고 있는 뷰그룹에게 있다. 따라서 자식이 없는 뷰일 경우 Layout 과정에서 Measure 단계만 구현하는 경우가 많다. 커스텀뷰를 만들기 위해서는 Measure 단계를 정확하게 이해하는 것이 가장 중요하다. 각 뷰의 정확한 크기를 알아야  뷰를 위치 시키고 그릴 수 있기 때문이다.

뷰의 기하(Geometry)

  1. 뷰의 기하는 사각형이다. 
      * [(left, top), (right, bottom)] or [(left, top), (width, height)] 로 사각형을 정의한다.
      * 안드로이드에서 뷰는 두 종류의 width와 height를 가지고 있다. 
          * Measured Width, Measured Height 는 뷰가 부모뷰 크기의 범위 내에서 가지고 싶어하는 너비와 높이이다.
          * Drawing Width, Drawing Height 는 뷰의 실제 너비와 높이로 뷰를 그리기 위해서 사용한다. 
              1. 안드로이드에서는 이 Drawing Width와 Drawing Height를 width와 height로 표기한다.
          * 따라서 Measured Width, Measured Height 는 Drawing Width, Drawing Height 와 다를 수 있다. 
              * 왜냐하면 뷰의 패딩, 마진 등을 고려하면 원하는 크기에서 패딩 및 마진 값을 빼야 하기 때문이다.
  2. 뷰의 정확한 위치와 크기는 언제 알 수 있는가? 
      * 안드로이드에서 뷰의 위치와 크기는 Layout 과정이 끝나야 정확히 계산된다.
      * 이 Layout 과정은 여러번 호출 될 수 있다.
      * 그렇기 때문에 Draw 과정이 시작되기 직전에 View의 정확한 위치와 크기를 알 수 있다. 
          1. 위치 얻기 : getLeft(), getTop()
          2. 크기 얻기 : getWidth(), getHeight()
      * 그렇다면 View의 Draw 과정이 시작되기 직전을 어떻게 알 수 있을까? 
          * ViewTreeObserver 사용 
              * 뷰의 크기를 알고자 하는 뷰에서 getViewTreeObserver() 를 통해서 ViewTreeObserver를 가져온다
              * OnPreDrawListener 를 등록해서 드로잉 직전에 리스너가 호출 되도록 한다.
          * 픽셀 단위의 뷰의 크기를 알 수 있다. 
              * <pre class="lang:java decode:true">CustomView mCustomView;

@Override
protected void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);
  setContentView(R.layout.activity_main);

  mCustomView = (CustomView)findViewById(R.id.mCustomView);
  mCustomView.getViewTreeObserver().addOnPreDrawListener(new ViewTreeObserver.OnPreDrawListener() {
      @Override
      public boolean onPreDraw() {
          mCustomView.getViewTreeObserver().removeOnPreDrawListener(this);
          Log.v("TAGTAG", "width : " + mCustomView.getWidth());
          Log.v("TAGTAG", "height : " + mCustomView.getHeight());
          return false;
      }
  });
}</pre>

  3. 위의 과정이 일어나는 순서는 Top -> Down 이다. 
      * 즉 부모부터 과정이 시작되고 자식으로 전파된다. 
          * 따라서 Layout 과정에서 부모뷰의 크기가 결정되고 나서 자식뷰의 Layout 과정이 시작된다.
          * 여기서 유추해 볼 수 있는 것은 부모뷰의 Measure Spec Mode 에 따라서 자식의 Layout 과정을 달리 해야 한다.
          * Measure Spec Mode 
              * EXACTLY     (고정)
              * AT_MOST     (가변 및 가변의 최대값)
              * UNSPECIFIED (정해지지 않음. Layout 과정 재발생.)
  4. 뷰의 너비와 높이는 둘 중 하나이다. 
      * 고정값이다.정 (EXACTLY) 
          * match_parent 또는 70dp 와 같은 수치 
              * match_parent 가 고정값인 이유는 Layout 과정이 Top-Down으로 일어나므로 자식뷰의 Layout 과정에서는 이미 부모뷰의 크기가 정해진 고정값과 같다.
      * 가변적이다. (AT_MOST) 
          * wrap_content
  5. 부모뷰의 Measure Spec Mode에 따른 자식뷰의 크기 정하는 규칙 
      * 아래와 같이 변수를 가정해 보자
      * 부모뷰의 크기 : parentSize
      * 부모뷰의 패딩 : parentPadding
      * 부모뷰의 컨텐츠 크기 : parentContentSize = parentSize &#8211; parentPadding
      * 자식뷰의 LayoutParams 이 
          * 고정된 수치 (70dp) : childSize 일 경우
          * MATCH_PARENT 일 경우
          * WRAP_CONTENT 일 경우
  6. <div class="table-responsive">
      <table  style="width:100%; "  class="easy-table easy-table-default " border="0">
        <caption>부모뷰가 EXACTLY 일 경우</caption> <tr>
          <th  style="text-align:center" >
            자식뷰레이아웃
          </th>
          
          <th  style="text-align:center" >
            뷰모드
          </th>
          
          <th  style="text-align:center" >
            크기
          </th>
          
          <th  style="text-align:center" >
            설명
          </th>
        </tr>
        
        <tr>
          <td  style="text-align:center" >
            정확한 수치 크기
          </td>
          
          <td  style="text-align:center" >
            EXACTLY
          </td>
          
          <td  style="text-align:center" >
            childSize
          </td>
          
          <td  style="text-align:center" >
            자식뷰는 특정 크기를 원한다.
          </td>
        </tr>
        
        <tr>
          <td  style="text-align:center" >
            MATCH_PARENT
          </td>
          
          <td  style="text-align:center" >
            EXACTLY
          </td>
          
          <td  style="text-align:center" >
            parentContentSize
          </td>
          
          <td  style="text-align:center" >
            자식뷰는 부모와 같은 크기를 원한다.
          </td>
        </tr>
        
        <tr>
          <td  style="text-align:center" >
            WRAP_CONTENT
          </td>
          
          <td  style="text-align:center" >
            AT_MOST
          </td>
          
          <td  style="text-align:center" >
            parentContentSize
          </td>
          
          <td  style="text-align:center" >
            자식뷰는 자신이 크기를 결정하기를 원한다. 즉 parentContentSize 가 최대값이다.
          </td>
        </tr>
      </table>
    </div>
    
    <div class="table-responsive">
      <table  style="width:100%; "  class="easy-table easy-table-default " border="0">
        <caption>부모뷰가 AT_MOST 일 경우</caption> <tr>
          <th  style="text-align:center" >
            자식뷰레이아웃
          </th>
          
          <th  style="text-align:center" >
            뷰모드
          </th>
          
          <th  style="text-align:center" >
            크기
          </th>
          
          <th  style="text-align:center" >
            설명
          </th>
        </tr>
        
        <tr>
          <td  style="text-align:center" >
            정확한 수치 크기
          </td>
          
          <td  style="text-align:center" >
            EXACTLY
          </td>
          
          <td  style="text-align:center" >
            childSize
          </td>
          
          <td  style="text-align:center" >
            자식뷰는 특정 크기를 원한다.
          </td>
        </tr>
        
        <tr>
          <td  style="text-align:center" >
            MATCH_PARENT
          </td>
          
          <td  style="text-align:center" >
            AT_MOST
          </td>
          
          <td  style="text-align:center" >
            parentContentSize
          </td>
          
          <td  style="text-align:center" >
            자식뷰는 부모와 같은 크기를 원한다. 하지만 아직 크기가 고정되지 않았다. 단 부모보다 크지 않게 제한한다.
          </td>
        </tr>
        
        <tr>
          <td  style="text-align:center" >
            WRAP_CONTENT
          </td>
          
          <td  style="text-align:center" >
            AT_MOST
          </td>
          
          <td  style="text-align:center" >
            parentContentSize
          </td>
          
          <td  style="text-align:center" >
            자식뷰는 자신이 크기를 결정하기를 원한다. 단 부모뷰의 크기보다 클 수는 없다. 즉 parentContentSize 가 최대값이다.
          </td>
        </tr>
      </table>
    </div>
    
    <div class="table-responsive">
      <table  style="width:100%; "  class="easy-table easy-table-default " border="0">
        <caption>부모뷰가 UNSPECIFIED 일 경우</caption> <tr>
          <th  style="text-align:center" >
            자식뷰레이아웃
          </th>
          
          <th  style="text-align:center" >
            뷰모드
          </th>
          
          <th  style="text-align:center" >
            크기
          </th>
          
          <th  style="text-align:center" >
            설명
          </th>
        </tr>
        
        <tr>
          <td  style="text-align:center" >
            정확한 수치 크기
          </td>
          
          <td  style="text-align:center" >
            EXACTLY
          </td>
          
          <td  style="text-align:center" >
            childSize
          </td>
          
          <td  style="text-align:center" >
            자식뷰는 특정 크기를 원한다.
          </td>
        </tr>
        
        <tr>
          <td  style="text-align:center" >
            MATCH_PARENT
          </td>
          
          <td  style="text-align:center" >
            UNSPECIFIED
          </td>
          
          <td  style="text-align:center" >
            아직 정해지지 않았다.
          </td>
          
          <td  style="text-align:center" >
            자식뷰는 부모와 같은 크기를 원한다. 자식뷰는 나중에 자신의 크기를 정할 수 있다.
          </td>
        </tr>
        
        <tr>
          <td  style="text-align:center" >
            WRAP_CONTENT
          </td>
          
          <td  style="text-align:center" >
            UNSPECIFIED
          </td>
          
          <td  style="text-align:center" >
            아직 정해지지 않았다.
          </td>
          
          <td  style="text-align:center" >
            자식뷰는 자신이 크기를 결정하기를 원한다. 자식뷰는 나중에 자신의 크기를 정할 수 있다.
          </td>
        </tr>
      </table>
    </div>

  7. 예제

  * 문자열 여러개를 뷰의 너비와 높이에 맞춰 360도 회전해서 그리는 커스텀뷰

<pre class="lang:java decode:true ">package kr.pe.burt.android.customview;

import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.util.AttributeSet;
import android.view.View;

public class CircleTextView extends View {

    private int mDefaultTextSize = 40;
    private int mTextColor = Color.BLUE;
    private Paint mPaint = makePaint(mTextColor);
    private Paint mTestPaint = makePaint(mTextColor);
    private String mMessage = "Android";

    public CircleTextView(Context context) {
        super(context);
    }

    public CircleTextView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    public CircleTextView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {

        int width = measureWidth(widthMeasureSpec);
        int height= measureHeight(heightMeasureSpec);
        int textWidth = width - getPaddingLeft() - getPaddingRight();
        int textHeight = height - getPaddingTop() - getPaddingBottom();
        adjustTextSizeToFit(Math.min(textWidth, textHeight));
        setMeasuredDimension(width, height);
    }

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        int viewWidth = getWidth();
        int viewHeight = getHeight();
        //: 중점으로 이동
        canvas.translate(viewWidth/2, viewHeight/2);
        for(int i=0; i&lt;10; i++) {
            canvas.drawText(mMessage, 0, 0, mPaint);
            canvas.rotate(36); // 36 * 10 = 360
        }
    }

    private int measureWidth(int measureSpec) {
        return (measureText(measureSpec, getPaddingLeft(), getPaddingRight()));
    }

    private int measureHeight(int measureSpec) {
        return (measureText(measureSpec, getPaddingTop(), getPaddingBottom()));
    }

    private int measureText(int measureSpec, int padding1, int paddind2) {
        int result = 0;
        // specMode는 현재뷰를 담고 있는 부모뷰의 스펙모드에 따른 현재 뷰의 스펙모드 
        int specMode = MeasureSpec.getMode(measureSpec);
        // specSize는 현재뷰를 담고 있는 부모뷰가 줄 수 있는 공간의 최대크기
        int specSize = MeasureSpec.getSize(measureSpec);
        if(specMode == MeasureSpec.EXACTLY) {
            result = specSize;
        } else {
            // Measure the text: twice text size plus padding
            result = 2 * (int)mPaint.measureText(mMessage) + padding1 + paddind2;
            if(specMode == MeasureSpec.AT_MOST) {
                // specSize가 최대값이므로
                // result와 specSize 중 최소값을 찾는다
                result = Math.min(result, specSize);
            }
        }
        // UNSPECIFIED 이면 0을 리턴
        return result;
    }

    private void adjustTextSizeToFit(int availableLength) {
        int textSize = 0;
        for(int i=1; i&lt;=10; i++) {
            textSize = i * 10;
            mTestPaint.setTextSize(textSize);
            int requiredLength = 2 * (int)mTestPaint.measureText(mMessage);
            if(requiredLength &gt; availableLength) {
                break;
            }
        }
        mPaint.setTextSize(Math.max(10, textSize - 10));
    }

    private Paint makePaint(int color) {
        Paint p = new Paint(Paint.ANTI_ALIAS_FLAG);
        p.setColor(color);
        p.setTextSize(mDefaultTextSize);
        return p;
    }

}</pre>

  * 정해진 Aspect Ratio 에 따라 뷰의 크기를 정하는 커스텀뷰

<pre class="lang:java decode:true  ">package kr.pe.burt.android.customview;

import android.content.Context;
import android.util.AttributeSet;
import android.view.View;

public class AspectRatioView extends View {
    private static final double VIEW_ASPECT_RATIO = 1.5f;
    private ViewAspectRatioMeasurer varm = new ViewAspectRatioMeasurer(VIEW_ASPECT_RATIO);
    public AspectRatioView(Context context) {
        super(context);
    }

    public AspectRatioView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    public AspectRatioView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        varm.measure(widthMeasureSpec, heightMeasureSpec);
        setMeasuredDimension(varm.getMeasuredWidth(), varm.getMeasuredHeight());
    }
}



/************************************************************************************************************************************/

package kr.pe.burt.android.customview;

import android.view.View;

public class ViewAspectRatioMeasurer {
    private double aspectRatio;
    private Integer measuredWidth = null;
    private Integer measuredHeight= null;


    public ViewAspectRatioMeasurer(double aspectRatio) {
        this.aspectRatio = aspectRatio;
    }

    public void measure(int widthMeasureSpec, int heightMeasureSpec) {
        measure(widthMeasureSpec, heightMeasureSpec, aspectRatio);
    }

    public void measure(int widthMeasureSpec, int heightMeasureSpec, double aspectRatio) {
        int widthMode = View.MeasureSpec.getMode(widthMeasureSpec);
        int widthSize = widthMode == View.MeasureSpec.UNSPECIFIED ? Integer.MAX_VALUE : View.MeasureSpec.getSize(widthMeasureSpec);
        int heightMode= View.MeasureSpec.getMode(heightMeasureSpec);
        int heightSize= heightMode == View.MeasureSpec.UNSPECIFIED ? Integer.MAX_VALUE : View.MeasureSpec.getSize(heightMeasureSpec);

        if(heightMode == View.MeasureSpec.EXACTLY &&
           widthMode == View.MeasureSpec.EXACTLY) {

            /**
             * Possibillity 1: Both width and height fixed
             */
            measuredWidth = widthSize;
            measuredHeight= heightSize;

        } else if(heightMode == View.MeasureSpec.EXACTLY) {
            /**
             * Possibillity 2: Width dynamic, height fixed
             */
            measuredWidth = (int)Math.min(widthSize, widthSize/aspectRatio);
            measuredHeight= (int)(measuredWidth * aspectRatio);
        } else if(widthMode == View.MeasureSpec.EXACTLY) {
            /**
             * Possibillity 3: Width fixed, height dynamic
             */
            measuredHeight = (int)Math.min(heightSize, widthSize/aspectRatio);
            measuredWidth= (int)(measuredHeight * aspectRatio);
        } else {
            /**
             * Possibility 4: Both width and height dynamic
             */
            if(widthSize &gt; heightSize * aspectRatio) {
                measuredHeight = heightSize;
                measuredWidth = (int)(measuredHeight * aspectRatio);
            } else {
                measuredWidth = widthSize;
                measuredHeight = (int)(measuredWidth / aspectRatio);
            }
        }
    }

    public int getMeasuredWidth() {
        if( measuredWidth == null ) {
            throw new IllegalStateException("You need to run measure() before trying to get measured dimentions");
        }
        return measuredWidth;
    }

    public int getMeasuredHeight() {
        if( measuredHeight == null ) {
            throw new IllegalStateException("You need to run measure() before trying to get measured dimentions");
        }
        return measuredHeight;
    }

}</pre>

8. 참고

  * [<span style="text-decoration: underline;">http://www.vogella.com/tutorials/AndroidCustomViews/article.html</span>](http://www.vogella.com/tutorials/AndroidCustomViews/article.html)
  * [<span style="text-decoration: underline;">https://thenewcircle.com/s/post/1663/tutorial_enhancing_android_ui_with_custom_views_dave_smith_video</span>](https://thenewcircle.com/s/post/1663/tutorial_enhancing_android_ui_with_custom_views_dave_smith_video)
  * [<span style="text-decoration: underline;">https://www.buzzingandroid.com/2012/11/easy-measuring-of-custom-views-with-specific-aspect-ratio/</span>](https://www.buzzingandroid.com/2012/11/easy-measuring-of-custom-views-with-specific-aspect-ratio/)