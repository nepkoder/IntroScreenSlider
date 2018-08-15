Step 1: Add colors

    <!-- Screens background -->
    <color name="bg_screen1">#b12515</color>
    <color name="bg_screen2">#FFBB00</color>
    <color name="bg_screen3">#74a443</color>

    <!-- dots active colors -->
    <color name="dot_active">#000000</color>


    <!-- dots inactive colors -->
    <color name="dot_inactive">#cccccc</color>
	
	
Step 2 : Add Strings

    <string name="next">Next</string>
    <string name="skip">Skip</string>
    <string name="start">Start</string>
    <string name="title_slide1">Connect the charger</string>
    <string name="desc_slide1">It will start automatically when charger is connected</string>
    <string name="title_slide2">Battery status update</string>
    <string name="desc_slide2">It will  keep you updated on battery charging status through notification and home screen widget</string>
    <string name="title_slide3">Alert on 100%</string>
    <string name="desc_slide3">It will alert you when battery reaches 100%</string>
	

Step 3 : Add Dimens

    <!-- Intro Screen -->
    <dimen name="dots_height">30dp</dimen>
    <dimen name="dots_bottom_margin">20dp</dimen>
    <dimen name="img_width">120dp</dimen>
    <dimen name="img_height">120dp</dimen>
    <dimen name="slide_title">30sp</dimen>
    <dimen name="slide_desc">16sp</dimen>
    <dimen name="desc_padding">40dp</dimen>

Step 4 : Add drawables

Step 5 : Add Activity Layout

<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_intro"
    android:layout_width="match_parent"
    android:layout_height="match_parent"

    tools:context=".IntroActivity">

    <android.support.v4.view.ViewPager
        android:id="@+id/view_pager"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

    <LinearLayout
        android:id="@+id/layoutDots"
        android:layout_width="match_parent"
        android:layout_height="@dimen/dots_height"
        android:layout_alignParentBottom="true"
        android:layout_marginBottom="@dimen/dots_bottom_margin"
        android:gravity="center"
        android:orientation="horizontal"></LinearLayout>

    <View
        android:layout_width="match_parent"
        android:layout_height="1dp"
        android:alpha=".5"
        android:layout_above="@id/layoutDots"
        android:background="@android:color/white" />

    <Button
        android:id="@+id/btn_next"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_alignParentRight="true"
        android:layout_alignParentEnd="true"
        android:background="@null"
        android:text="@string/next"
        android:onClick="btnNextClick"
        android:textColor="@android:color/white" />

    <Button
        android:id="@+id/btn_skip"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_alignParentStart="true"
        android:layout_alignParentLeft="true"
        android:background="@null"
        android:text="@string/skip"
        android:onClick="btnSkipClick"
        android:textColor="@android:color/white" />

</RelativeLayout>

Step 6 : Add Slide Layouts

<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/bg_screen1">

    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:gravity="center_horizontal"
        android:orientation="vertical">

        <ImageView
            android:layout_width="@dimen/img_width"
            android:layout_height="@dimen/img_height"
            android:src="@drawable/ic_slide1" />

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/title_slide1"
            android:textColor="@android:color/white"
            android:textSize="@dimen/slide_title"
            android:textStyle="bold" />

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="20dp"
            android:paddingLeft="@dimen/desc_padding"
            android:paddingRight="@dimen/desc_padding"
            android:text="@string/desc_slide1"
            android:textAlignment="center"
            android:textColor="@android:color/white"
            android:textSize="@dimen/slide_desc" />

    </LinearLayout>

</RelativeLayout>	


Step 7 : Create class extending PagerAdapter

 public class ViewPagerAdapter extends PagerAdapter {
        private LayoutInflater layoutInflater;


        public ViewPagerAdapter() {

        }

        @Override
        public Object instantiateItem(ViewGroup container, int position) {
            layoutInflater = (LayoutInflater) getSystemService(Context.LAYOUT_INFLATER_SERVICE);

            View view = layoutInflater.inflate(layouts[position], container, false);
            container.addView(view);

            return view;
        }

        @Override
        public int getCount() {
            return layouts.length;
        }

        @Override
        public boolean isViewFromObject(View view, Object obj) {
            return view == obj;
        }


        @Override
        public void destroyItem(ViewGroup container, int position, Object object) {
            View view = (View) object;
            container.removeView(view);
        }
    }

   
Step 8 : Delcare objects

    private ViewPager viewPager;
    private ViewPagerAdapter viewPagerAdapter;
    private LinearLayout dotsLayout;
    private TextView[] dots;
    private int[] layouts;
    private Button btnSkip, btnNext;

	
	
Sttep 9 : Add methods
 
  
    private void addBottomDots(int currentPage) {
        dots = new TextView[layouts.length];

        dotsLayout.removeAllViews();
        for (int i = 0; i < dots.length; i++) {
            dots[i] = new TextView(this);
            dots[i].setText(Html.fromHtml("&#8226;"));
            dots[i].setTextSize(35);
            dots[i].setTextColor(getResources().getColor(R.color.dot_inactive));
            dotsLayout.addView(dots[i]);
        }

        if (dots.length > 0)
            dots[currentPage].setTextColor(getResources().getColor(R.color.dot_active));
    }



    private int getItem(int i) {
        return viewPager.getCurrentItem() + i;
    }

    private void launchHomeScreen() {
        startActivity(new Intent(this, MainActivity.class));
        finish();
    }
	
	
Step 10 : Add PageChangeLister for PageAdapter

  ViewPager.OnPageChangeListener viewPagerPageChangeListener = new ViewPager.OnPageChangeListener() {

        @Override
        public void onPageSelected(int position) {
            addBottomDots(position);

            // changing the next button text 'NEXT' / 'GOT IT'
            if (position == layouts.length - 1) {
                // last page. make button text to GOT IT
                btnNext.setText(getString(R.string.start));
                btnSkip.setVisibility(View.GONE);
            } else {
                // still pages are left
                btnNext.setText(getString(R.string.next));
                btnSkip.setVisibility(View.VISIBLE);
            }
        }

        @Override
        public void onPageScrolled(int arg0, float arg1, int arg2) {

        }

        @Override
        public void onPageScrollStateChanged(int arg0) {

        }
    };


Step 10 : Make activity fullscreen

Change activty extends from AppCompatActivity to Activity

and this code above  setContentView(R.layout.activity_intro);

 requestWindowFeature(Window.FEATURE_NO_TITLE);
        getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN, WindowManager.LayoutParams.FLAG_FULLSCREEN);
       
	
Step 11: Initalize all objects in OnCreate

viewPager = (ViewPager) findViewById(R.id.view_pager);
        dotsLayout = (LinearLayout) findViewById(R.id.layoutDots);
        btnSkip = (Button) findViewById(R.id.btn_skip);
        btnNext = (Button) findViewById(R.id.btn_next);

        layouts = new int[]{
                R.layout.intro_slide1,
                R.layout.intro_slide2,
                R.layout.intro_slide3};

        // adding bottom dots
        addBottomDots(0);

        viewPagerAdapter = new ViewPagerAdapter();
        viewPager.setAdapter(viewPagerAdapter);
        viewPager.addOnPageChangeListener(viewPagerPageChangeListener);
		
		
Step 12 : Add Click Listener for both buttons

 public  void btnSkipClick(View v)
    {
        launchHomeScreen();
    }

    public  void btnNextClick(View v)
    {
        // checking for last page
        // if last page home screen will be launched
        int current = getItem(1);
        if (current < layouts.length) {
            // move to next screen
            viewPager.setCurrentItem(current);
        } else {
            launchHomeScreen();
        }
    }

Step 13 : Set Click Listner on buttons
