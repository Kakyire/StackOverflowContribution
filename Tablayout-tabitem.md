
This can be solve programmatically 

**Step 1** Create `FragmentStateAdapter` adapter class

```java
public class TabAdapter extends FragmentStateAdapter {

    public TabAdapter(@NonNull FragmentActivity fragmentActivity) {
        super(fragmentActivity);
    }

    @NonNull
    @Override
    public Fragment createFragment(int position) {

        switch (position) {
            case 0:
                return new DayFragment();
            case 1:
                return new MyExerciseFragment();
            default:
                return new ExerciseFragment();
        }
    }

    @Override
    public int getItemCount() {
        return 3;
    }
}

```

**Step 2** Create a method in your `activity` or `fragment` class to set tabs using `TabLayoutMediator` and call the method in `onCreate` method

```java
void setupTabLayout() {
        ViewPager2 viewPager = binding.exerciseMenuViewPager;
        
        viewPager.setAdapter(tabAdapter);

        TabLayout tabLayout = binding.exerciseMenuTabLayout;

        new TabLayoutMediator(tabLayout, viewPager, (tab, position) -> {
            switch (position) {
                case 0:
                    tab.setText("Day");
                    break;
                case 1:
                    tab.setText("My Exercise");
                    break;
                default:
                    tab.setText("Exercise");
            }
        }).attach();
    }
```


This is how your activity class will look like

```java
public class MainActivity extends AppCompatActivity {


    private ActivityMainBinding binding;
    private TabAdapter tabAdapter;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        binding = ActivityMainBinding.inflate(getLayoutInflater());
        setContentView(binding.getRoot());

        tabAdapter = new TabAdapter(this);

        setupTabLayout();
    }

    void setupTabLayout() {}
}
```

**NOTE:** To use *ViewBinding* add this code to your app level `build.gradle` file.

```gradle
android{
    
    ...
     buildFeatures {
        viewBinding true
    }
    ...
}
```

**`activity_main.xml`**
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <com.google.android.material.tabs.TabLayout
        android:id="@+id/exercise_menu_tabLayout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:layout_constraintTop_toTopOf="parent" />

    <androidx.viewpager2.widget.ViewPager2
        android:id="@+id/exercise_menu_viewPager"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:layout_constraintTop_toBottomOf="@id/exercise_menu_tabLayout" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

[Final results](https://user-images.githubusercontent.com/47930771/108611496-ee1cea00-73d6-11eb-9dab-e6cfd78e48b9.png)




I hope this helps

***PS:*** You need to create these fragment classes `DayFragment.java`, `MyExerciseFragment.java`and `ExerciseFragment`.