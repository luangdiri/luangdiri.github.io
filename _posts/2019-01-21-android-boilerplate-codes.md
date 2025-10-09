---
title: "Android Boilerplate Codes"
date: 2019-01-21 13:51:00
update_at: 2020-05-26 13:02:28
tags:
- programming
---

Started writing the draft of this post on Random Wiki [Issue #7](https://github.com/aemxn/random-wiki/issues/7). Delays after delays, it never gets published. Only certain part is already published on this blog.

This is a very tl;dr post about Android boilerplate codes. Boilerplate codes are code snippets you can use repeatedly on different projects. Note that these snippets are ~2 years old, back when I'm still working as an Android developer (I still do and wants more of it) and as far as I can remember, during Android Nougat era. So expect legacy libraries being used here and there.

This is like my "notepad" on Android development. I refer to these snippets every time I'm building a new project or forgets how to do certain things. There are less explanations on how the code works, so use it if you know what you're doing.

---

### Contents:

- [Horizontal List](#horizontal-list)
    - [1. Basic list](#1-basic-list)
      - [Layout](#layout)
      - [Activity](#activity)
      - [Fragment](#fragment)
      - [Adapter](#adapter)
    - [2. Basic list with click listener (with ActionMode)](#2-basic-list-with-click-listener-with-actionmode)
      - [Required variables](#required-variables)
      - [Interface](#interface)
      - [Passing listener to adapter](#passing-listener-to-adapter)
      - [Overriding methods](#overriding-methods)
      - [Custom method to toggle selection (multi-select)](#custom-method-to-toggle-selection-multi-select)
      - [ActionMode callback](#actionmode-callback)
      - [If in viewpager, disable the ActionMode in other tabs](#if-in-viewpager-disable-the-actionmode-in-other-tabs)
    - [Implementation in the Adapter](#implementation-in-the-adapter)
      - [Required variables](#required-variables-1)
      - [Initialize in constructor](#initialize-in-constructor)
      - [Custom methods on handling selection](#custom-methods-on-handling-selection)
      - [Setting listener (retrieve instance from Fragment/Activity)](#setting-listener-retrieve-instance-from-fragmentactivity)
      - [Passing listener to ViewHolder](#passing-listener-to-viewholder)
      - [Implementing in ViewHolder](#implementing-in-viewholder)
    - [3. RecyclerView with a Header and an empty view](#3-recyclerview-with-a-header-and-an-empty-view)
      - [Necessary variables](#necessary-variables)
      - [Inflate views](#inflate-views)
      - [Getting data set size](#getting-data-set-size)
      - [Assign view type](#assign-view-type)
      - [Using the viewholder individually](#using-the-viewholder-individually)
- [Handling Scrolls with CoordinatorLayout](#handling-scrolls-with-coordinatorlayout)
      - [XML layout](#xml-layout)
      - [Custom ImageView class](#custom-imageview-class)
      - [Behavior class](#behavior-class)
      - [Necessary view utils for compatibility](#necessary-view-utils-for-compatibility)
- [Passing Data Through App](#passing-data-through-app)
  - [1. Normal intent](#1-normal-intent)
  - [2. Using Serializable](#2-using-serializable)
      - [Make our POJO class implements Serializable](#make-our-pojo-class-implements-serializable)
      - [Sending our POJO to another activity](#sending-our-pojo-to-another-activity)
      - [Receiving our POJO intent](#receiving-our-pojo-intent)
  - [3. Using Parcelable](#3-using-parcelable)
      - [Make our POJO implements Parcelable](#make-our-pojo-implements-parcelable)
      - [Receiving our POJO intent](#receiving-our-pojo-intent-1)
  - [4. Using EventBus](#4-using-eventbus)
      - [Creating our bus](#creating-our-bus)
      - [Creating our event](#creating-our-event)
      - [Creating our sender](#creating-our-sender)
      - [Creating our subscriber (retriever)](#creating-our-subscriber-retriever)
      - [Protip](#protip)
      - [Resources](#resources)
- [Basic ViewPager With Tabs](#basic-viewpager-with-tabs)
      - [XML layout](#xml-layout-1)
      - [Custom styling](#custom-styling)
      - [Java](#java)
      - [Adapter](#adapter-1)
    - [Modifications](#modifications)
      - [1. Lock swiping](#1-lock-swiping)
- [Basic ViewPager Using PagerAdapter](#basic-viewpager-using-pageradapter)
      - [XML Layout](#xml-layout-2)
      - [Java](#java-1)
- [Using Fragment in an Activity](#using-fragment-in-an-activity)
    - [1. FragmentManager add/replace FrameLayout ID](#1-fragmentmanager-addreplace-framelayout-id)
    - [2. Using newInstance (passing data)](#2-using-newinstance-passing-data)
      - [Activity](#activity-1)
      - [Fragment](#fragment-1)
      - [Retrieving the Bundle args](#retrieving-the-bundle-args)
- [Building a Preference Page](#building-a-preference-page)
    - [1. Activity](#1-activity)
      - [Layout](#layout-1)
      - [Java](#java-2)
    - [2. PreferenceFragment](#2-preferencefragment)
      - [Layout](#layout-2)
      - [Java](#java-3)
- [Long Running Task](#long-running-task)
    - [AsyncTask](#asynctask)
    - [Thread](#thread)
    - [Handler with delay](#handler-with-delay)
- [Runtime Permission](#runtime-permission)
      - [Required variable](#required-variable)
      - [Initializing the permission.](#initializing-the-permission)
      - [Receiving permission result](#receiving-permission-result)
- [Optimizing Large Bitmaps](#optimizing-large-bitmaps)
- [Displaying Notifications](#displaying-notifications)
      - [Notification with a button and a click listener](#notification-with-a-button-and-a-click-listener)
- [Android Logging Utility](#android-logging-utility)
    - [Toasting](#toasting)
    - [Snackbar](#snackbar)

---

# Horizontal List

### 1. Basic list
#### Layout

**Note:**
- If implementing in Fragment, doesn't need `toolbar`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent">

    <include
            android:id="@+id/toolbar"
            layout="@layout/toolbar" />

    <android.support.v7.widget.RecyclerView
            android:id="@+id/rv_receipts"
            android:layout_width="match_parent"
            android:layout_height="wrap_content" />

</LinearLayout>
```
#### Activity

**Note:**
- Fragment doesn't need `toolbar` as it will inherit from the parent activity.
- Fragment doesn't need `onBackPressed()`

```java
public class ActivityClassName extends AppCompatActivity {
    
        @BindView(R.id.toolbar)
            Toolbar toolbar;
            @BindView(R.id.rv_receipts)
            RecyclerView mRecyclerView;

    public void onCreate() {
                ...
            initToolbar();
            initRecyclerView();
        }

    private void initToolbar() {
                toolbar.setTitle("Toolbar title");
            setSupportActionBar(toolbar);
            getSupportActionBar().setDisplayHomeAsUpEnabled(true);
            getSupportActionBar().setHomeButtonEnabled(false);
        }

    private void initRecyclerView() {
    
            // Vertical list
                    LinearLayoutManager manager = new LinearLayoutManager(getActivity());
                    manager.setOrientation(LinearLayoutManager.VERTICAL);

       // Horizontal list
              // LinearLayoutManager layoutManager = new LinearLayoutManager(this, LinearLayoutManager.HORIZONTAL, false);

       // StaggeredGrid list
              // First param is number of columns and second param is orientation i.e Vertical or Horizontal
              // StaggeredGridLayoutManager gridLayoutManager = new StaggeredGridLayoutManager(2, StaggeredGridLayoutManager.VERTICAL);

       // Decorations (row divider)
              // RecyclerView.ItemDecoration itemDecoration = new DividerItemDecoration(this, DividerItemDecoration.VERTICAL_LIST);
              // mRecyclerView.addItemDecoration(itemDecoration);

        mRecyclerView.setLayoutManager(manager);
                mRecyclerView.setHasFixedSize(true);

        YourListAdapter mListAdapter = new YourListAdapter();
                mRecyclerView.setAdapter(mListAdapter);
                mListAdapter.add(mYourDataset);
            }

    @Override
        public boolean onOptionsItemSelected(MenuItem item) {
                switch (item.getItemId()) {
                    case android.R.id.home:
                    onBackPressed();
                    return true;
                default:
                    return false;
            }
        }
    }
```

#### Fragment

```java
public class FragmentClassName extends Fragment {
    
        @BindView(R.id.rv_receipts)
            RecyclerView mRecyclerView;

    public void onCreateView() {
                initRecyclerView();
        }

    private void initRecyclerView() {
    
            // Vertical list
                    LinearLayoutManager manager = new LinearLayoutManager(getActivity());
                    manager.setOrientation(LinearLayoutManager.VERTICAL);
                    mRecyclerView.setLayoutManager(manager);
                    mRecyclerView.setHasFixedSize(true);

        YourListAdapter mListAdapter = new YourListAdapter();
                mRecyclerView.setAdapter(mListAdapter);
                mListAdapter.add(mYourDataset);
            }
        }
```

#### Adapter

```java
public class YourListAdapter extends RecyclerView.Adapter<RecyclerView.ViewHolder> {
    
        private List<Object> mYourDataset = new ArrayList<>();

    public YourListAdapter() {
                // constructor
        }

    // custom dataset initialization
        public void add(List<Object> mYourDataset) {
                this.mYourDataset = mYourDataset;
            notifyDataSetChanged();
        }

    @Override
        public RecyclerView.ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
                LayoutInflater inflater = LayoutInflater.from(parent.getContext());
            View mRowView = inflater.inflate(R.layout.row_view, parent, false);
            return new YourListViewHolder(mRowView);
        }

    @Override
        public void onBindViewHolder(RecyclerView.ViewHolder holder, int position) {
                YourListViewHolder yourListVH = (YourListViewHolder) holder;
            Object item = mYourDataset.get(position);
            yourListVH.bind(item);
        }

    @Override
        public int getItemCount() {
                return mYourDataset.size();
        }

    static class YourListViewHolder extends RecyclerView.ViewHolder {
    
            public YourListViewHolder(View itemView) {
                            super(itemView);
                        // initialize row view components
                    }

        public void bind(final Object item) {
                        // row view implementation
                }
            }
        }
```
### 2. Basic list with click listener (with ActionMode)
#### Required variables

```java
ActionMode mActionMode;
Integer selectedPosition = null;
boolean isInActionMode = false;
```
#### Interface

```java
public interface OnItemClickListener {
        void onItemClick(View v, int position);
}
```

```java
public interface OnItemLongClickListener {
        void onItemLongClick(View v, int position);
}

```
#### Passing listener to adapter

```java
mListAdapter.setOnItemClickListener(this);
mListAdapter.setOnItemLongClickListener(this);
```
#### Overriding methods

Retrieving request listener request from the implementor (adapter)

```java
@Override
public void onItemLongClick(View v, int position) {
        v.setSelected(true);
    selectedPosition = position;
    mActionMode = ((AppCompatActivity) getActivity()).startSupportActionMode(mActionModeCallback);
    toggleSelection(position);
}

@Override
public void onItemClick(View v, int position, String title) {
        if (isInActionMode) {
            // in multi select mode
        toggleSelection(position);
    } else {
            // do something on single click (and not in actionmode)
    }
}
```
#### Custom method to toggle selection (multi-select)

```java
public void toggleSelection(int idx) {
        mListAdapter.toggleSelection(idx);
    String title = getString(R.string.selected_count, mListAdapter.getSelectedItemCount());
    mActionMode.setTitle(title);
}
```
#### ActionMode callback

```java

private ActionMode.Callback mActionModeCallback = new ActionMode.Callback() {
        @Override
    public boolean onCreateActionMode(ActionMode mode, Menu menu) {
            mode.setTitle("ActionMode title");
        // set an action menu item
        mode.getMenuInflater().inflate(R.menu.menu_deals_cab, menu);
        // changing color of viewpagertab and toolbar when in actionmode
        getActivity().findViewById(R.id.viewpagertab).setBackgroundColor(ContextCompat.getColor(getActivity(), R.color.color_primary_dark));
        getActivity().findViewById(R.id.toolbar).setBackgroundColor(ContextCompat.getColor(getActivity(), R.color.color_primary_dark));
        isInActionMode = true;
        return true;
    }

    @Override
        public boolean onPrepareActionMode(ActionMode mode, Menu menu) {
                return false;
        }

    @Override
        public boolean onActionItemClicked(ActionMode mode, MenuItem item) {
                // handle on menu item clicked
            if (mListAdapter.getSelectedItems().size() > 0) {
                    switch (item.getItemId()) {
                        case R.id.action_done:
                        initSelectedItems();
                        return true;
                    default:
                        return false;
                }
            } else {
                    T.showS(getActivity(), "No item selected");
                return false;
            }
        }

    @Override
        public void onDestroyActionMode(ActionMode mode) {
                // reset everything on disabling actionmode
            isInActionMode = false;
            selectedPosition = null;
            getActivity().findViewById(R.id.viewpagertab).setBackgroundColor(ContextCompat.getColor(getActivity(), R.color.color_primary));
            getActivity().findViewById(R.id.toolbar).setBackgroundColor(ContextCompat.getColor(getActivity(), R.color.color_primary));
            if (mListAdapter != null) {
                    mListAdapter.clearSelections();
            } else {
                    T.showS(getActivity(), "No deals selected");
            }
        }
    };

```
#### If in viewpager, disable the ActionMode in other tabs

```java
@Override
public void setUserVisibleHint(boolean isVisibleToUser) {
        super.setUserVisibleHint(isVisibleToUser);
    if (this.isVisible()) {
            if (!isVisibleToUser) {
                if (mActionMode != null) {
                    mActionMode.finish();
            }
        }
    }
}
```
### Implementation in the Adapter
#### Required variables

```java
SparseBooleanArray selectedItems;
OnItemClickListener onItemClickListener;
OnItemLongClickListener onItemLongClickListener;
```
#### Initialize in constructor

```java
public YourListAdapter() {
        // some necessary stuff to initialize
    selectedItems = new SparseBooleanArray();
}
```
#### Custom methods on handling selection

```java
// toggling selections and update view
public void toggleSelection(int pos) {
        if (selectedItems.get(pos, false)) {
            selectedItems.delete(pos);
    } else {
            selectedItems.put(pos, true);
    }
    notifyItemChanged(pos);
}

public int getSelectedItemCount() {
        return selectedItems.size();
}

public void clearSelections() {
        selectedItems.clear();
    notifyDataSetChanged();
}

// getting data from selected rows
public List<Integer> getSelectedItems() {
        List<Integer> items = new ArrayList<>(selectedItems.size());
    for (int i = 0; i < selectedItems.size(); i++) {
            items.add(selectedItems.keyAt(i));
    }
    return items;
}
```
#### Setting listener (retrieve instance from Fragment/Activity)

This is to ensure we can click on individual rows of the list.

```java
public void setOnItemClickListener(OnItemClickListener onItemClickListener) {
        this.onItemClickListener = onItemClickListener;
}

public void setOnItemLongClickListener(OnItemLongClickListener onItemLongClickListener) {
        this.onItemLongClickListener = onItemLongClickListener;
}
```
#### Passing listener to ViewHolder

```java
@Override
public void onBindViewHolder(RecyclerView.ViewHolder holder, int position) {
            YourListViewHolder yourListVH = (YourListViewHolder) holder;
        Object item = mYourDataset.get(position);

        // passing listener variable to viewholder
                yourListVH.bind(item, onItemClickListener, onItemLongClickListener);
                // one way to update variable in viewholder besides passing in public method (bind())
                yourListVH.position = position;
                 // this is where the row view UI get updated
                yourListVH.itemView.setActivated(selectedItems.get(position, false));
        }
```
#### Implementing in ViewHolder
        
Notice, using the native listener.

```java
static class YourListViewHolder extends RecyclerView.ViewHolder implements View.OnClickListener, View.OnLongClickListener {
    
        // ... other variables

    OnItemClickListener mClickListener;
        OnItemLongClickListener mLongClickListener;

    // for view update (on multi select mode)    
        int position;

    // ... constructor and other stuff

    public void bind(final Object item, OnItemClickListener mClickListener, OnItemLongClickListener mLongClickListener) {
                this.mClickListener = mClickListener;
            this.mLongClickListener = mLongClickListener;

        // ... other view manipulation stuff

        itemView.setOnClickListener(this);
                itemView.setOnLongClickListener(this);
            }

    @Override
        public void onClick(View v) {
                if (mClickListener != null) {
                    mClickListener.onItemClick(v, position);
            }
        }

    @Override
        public boolean onLongClick(View v) {
                if (mLongClickListener != null) {
                    mLongClickListener.onItemLongClick(v, position);
                return true;
            }
            return false;
        }
    }
```
### 3. RecyclerView with a Header and an empty view

This will require Adapter lifecycle knowledge.

The main idea of getting an Empty View is to know if the data set size passed into the adapter is 0 hence, inflate empty view.

For a Header to be displayed, we will set the first iteration to inflate a header view in the adapter.

We will skip the `mYourDataset` passing process.
#### Necessary variables

Used for ViewHolder identifier

```java
static final int HEADER = 1;
static final int ITEM_LIST = 2;
static final int EMPTY_LIST = 3;
```
#### Inflate views

```java
@Override
public RecyclerView.ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
    
        LayoutInflater inflater = LayoutInflater.from(parent.getContext());
            switch (viewType) {
                    case ITEM_LIST:
                    View mRowView = inflater.inflate(R.layout.row_view, parent, false);
                    return new YourListViewHolder(mRowView);
                case EMPTY_LIST:
                    View emptyView = inflater.inflate(R.layout.empty_layout, parent, false);
                    return new EmptyViewHolder(emptyView);
                default:
                case HEADER:
                    View headerView = inflater.inflate(R.layout.header_layout, parent, false);
                    return new HeaderViewHolder(headerView);
            }
        }
```
#### Getting data set size

In order for this adapter to iterate and inflate individual rows, it needs to know how big our data set is.

```java
    @Override
    public int getItemCount() {
            if (mYourDataset.size() > 1) {
                return mYourDataset.size() + 1;
        } else {
                return 2; // header and empty view
        }
    }
```
#### Assign view type

To be used by `onCreateViewHolder()` to know which `viewType` is currently interating.

This method will control which position the view will be inflated.

`position` is retrieved from `getItemCount()` method.

Condition: We want the Header to always be displayed and the Empty view only display if the dataset is empty.

```java
    @Override
    public int getItemViewType(int position) {
            if (mYourDataset.size() >= 1) {
                return (position == 0 ? HEADER : ITEM_LIST);
        } else {
                return (position == 0 ? HEADER : EMPTY_LIST);
        }
    }
```
#### Using the viewholder individually

Notice, we are getting `position - 1` in our data set. Why? Because we have told the adapter to always use position 1 as header, thus during iteration, our position will be incremented by 1.

```java
@Override
public void onBindViewHolder(RecyclerView.ViewHolder holder, int position) {
    
        if (holder instanceof YourListAdapter) {
                    YourListViewHolder yourListVH = (YourListViewHolder) holder;
                Object item = mYourDataset.get(position - 1);
                yourListVH.bind(item);
            } else if (holder instanceof HeaderViewHolder) {
                    HeaderViewHolder headerVh = (HeaderViewHolder) holder;
                headerVh.bind();
            } else if (holder instanceof DealsEmptyViewHolder) {
                    DealsEmptyViewHolder dealsEmptyVh = (DealsEmptyViewHolder) holder;
                dealsEmptyVh.bind();
            }

}
```

# Handling Scrolls with CoordinatorLayout

#### XML layout

We set up our layout in a CoordinatorLayout.

To take advantage of the new feature in Material Design library, we put an AppBarLayout which is an enhanced version of a LinearLayout; includes feature like scrolling gestures.

What we would expect this layout will do is when user scrolls, the ImageView in the CollapsingToolbarLayout will have a parallax effect.

NestedScrollView is just like ScrollView, but it supports acting as both a nested scrolling parent and child on both new and old versions of Android. Nested scrolling is enabled by default. 

We put our custom ImageView under CoordinatorLayout child to make it float between the AppBarLayout and NestedScrollView (using layout_anchor attribute to determine its position).

This method can be applied in FAB button to achieve the same effect as well.

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/coordinatorLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true">

    <!-- Collapsible header with built-in toolbar -->

    <android.support.design.widget.AppBarLayout
            android:id="@+id/appbar"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
            android:fitsSystemWindows="true">

        <android.support.design.widget.CollapsingToolbarLayout
                    android:id="@+id/collapsing_toolbar"
                    android:layout_width="match_parent"
                    android:layout_height="match_parent"
                    android:fitsSystemWindows="true"
                    app:contentScrim="?attr/colorPrimary"
                    app:layout_scrollFlags="scroll|exitUntilCollapsed|snap"
                    app:titleEnabled="false">

            <ImageView
                            android:id="@+id/iv_placeholder"
                            android:layout_width="match_parent"
                            android:layout_height="wrap_content"
                            android:fitsSystemWindows="true"
                            android:scaleType="centerCrop"
                            android:src="@drawable/header_bg"
                            app:layout_collapseMode="parallax"
                            app:layout_collapseParallaxMultiplier="0.9" />

            <android.support.v7.widget.Toolbar
                            android:id="@+id/toolbar"
                            android:layout_width="match_parent"
                            android:layout_height="?attr/actionBarSize"
                            app:layout_collapseMode="pin"
                            app:popupTheme="@style/ThemeOverlay.AppCompat.Light" />

        </android.support.design.widget.CollapsingToolbarLayout>
            </android.support.design.widget.AppBarLayout>

    <!-- Our view contents -->

    <android.support.v4.widget.NestedScrollView
            android:id="@+id/nested_sv"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:layout_behavior="@string/appbar_scrolling_view_behavior" >

        <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:orientation="vertical"
                    android:paddingTop="58dp">

            <!-- Other Views -->

        </LinearLayout>
            </android.support.v4.widget.NestedScrollView>

    <com.aemxn.com.ui.MyCustomImageView
            android:id="@+id/iv"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:src="@drawable/deals_history_circle"
            app:layout_anchor="@id/appbar"
            app:layout_anchorGravity="bottom|center"/>

</android.support.design.widget.CoordinatorLayout>
```

#### Custom ImageView class

Ensure our layout make use of CoordinatorLayout etc, in order to use this feature.

There are two ways to apply this behavior into our ImageView:
1. Apply in xml

   Use `layout_behavior` attribute into our custom ImageView class

```xml
<com.aemxn.com.ui.MyCustomImageView
    android:id="@+id/iv"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:src="@drawable/deals_history_circle"
    app:layout_anchor="@id/appbar"
    app:layout_anchorGravity="bottom|center"
    app:layout_behavior="com.aemxn.ui.MyCustomImageViewBehavior" />
```
1. Apply in java for more control

   Notice the first line, we are using an annotation to implement behavior to this view class.

   Once it is implemented, our custom ImageView will inherit the behavior set in the custom behavior class.

```java
@CoordinatorLayout.DefaultBehavior(MyCustomImageViewBehavior.class)
public class MyCustomImageView extends ImageView {
        public MyCustomImageView(Context context) {
            super(context);
    }

    public MyCustomImageView(Context context, AttributeSet attrs) {
                super(context, attrs);
        }

    public MyCustomImageView(Context context, AttributeSet attrs, int defStyleAttr) {
                super(context, attrs, defStyleAttr);
        }

    public MyCustomImageView(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) {
                super(context, attrs, defStyleAttr, defStyleRes);
        }
    }
```

#### Behavior class

For detailed explanation: https://www.bignerdranch.com/blog/customizing-coordinatorlayouts-behavior/

We can pass any view we want into the Behavior inheritance, in this case we pass ImageView.

We would expect our custom ImageView to be hidden when we scroll up, and show it again when scrolling down.

Below implementation is imitated from the native FAB behavior code.

```java
public class MyCustomImageViewBehavior extends CoordinatorLayout.Behavior<ImageView> {
    
        private Rect mTmpRect;

    public MyCustomImageViewBehavior() {
                super();
        }

    public MyCustomImageViewBehavior(Context context, AttributeSet attrs) {
                super(context, attrs);
        }

    @Override
        public boolean layoutDependsOn(CoordinatorLayout parent, ImageView child, View dependency) {
    
            // check that our dependency is the AppBarLayout
                    return dependency instanceof AppBarLayout;
                }

    @Override
        public boolean onDependentViewChanged(CoordinatorLayout parent, ImageView child,
                                                        View dependency) {
                if (dependency instanceof AppBarLayout) {
                    return updateImageViewVisibility(parent, (AppBarLayout) dependency, child);
            }
            return false;
        }

    private boolean updateImageViewVisibility(CoordinatorLayout parent, AppBarLayout appBarLayout, final ImageView child) {
                final CoordinatorLayout.LayoutParams lp = (CoordinatorLayout.LayoutParams) child.getLayoutParams();

        if (lp.getAnchorId() != appBarLayout.getId()) {
                        // The anchor ID doesn't match the dependency, so we won't automatically
                    // show/hide the FAB
                    return false;
                }

        if (mTmpRect == null) {
                        mTmpRect = new Rect();
                }

        // First, let's get the visible rect of the dependency
                final Rect rect = mTmpRect;
                ViewGroupUtils.getDescendantRect(parent, appBarLayout, rect);

        // calculation to make sure when the imageview is touching the bottom edge of the toolbar
                if ((rect.bottom - (appBarLayout.getHeight() / 2)) <= (appBarLayout.getHeight() / 4)) {
                        // If the anchor's bottom is below the seam, hide it
                    child.animate()
                            .translationY(0)
                            .alpha(0.0f)
                            .setListener(new AnimatorListenerAdapter() {
                                    @Override
                                public void onAnimationEnd(Animator animation) {
                                        super.onAnimationEnd(animation);
                                    child.setVisibility(View.INVISIBLE);
                                }
                            });
                } else {
                        // Else, show it
                    child.animate()
                            .translationY(0)
                            .alpha(1.0f)
                            .setListener(new AnimatorListenerAdapter() {
                                    @Override
                                public void onAnimationEnd(Animator animation) {
                                        super.onAnimationEnd(animation);
                                    child.setVisibility(View.VISIBLE);
                                }
                            });
                }
                return true;
            }
        }
```

#### Necessary view utils for compatibility

```java
public class ViewGroupUtils {
        private static final ViewGroupUtils.ViewGroupUtilsImpl IMPL;

    ViewGroupUtils() {

        }

    public static void offsetDescendantRect(ViewGroup parent, View descendant, Rect rect) {
                IMPL.offsetDescendantRect(parent, descendant, rect);
        }

    public static void getDescendantRect(ViewGroup parent, View descendant, Rect out) {
                out.set(0, 0, descendant.getWidth(), descendant.getHeight());
            offsetDescendantRect(parent, descendant, out);
        }

    static {
                int version = Build.VERSION.SDK_INT;
            if(version >= 11) {
                    IMPL = new ViewGroupUtils.ViewGroupUtilsImplHoneycomb();
            } else {
                    IMPL = new ViewGroupUtils.ViewGroupUtilsImplBase();
            }

    }

    private static class ViewGroupUtilsImplHoneycomb implements ViewGroupUtils.ViewGroupUtilsImpl {
                private ViewGroupUtilsImplHoneycomb() {

            }

        public void offsetDescendantRect(ViewGroup parent, View child, Rect rect) {
                        ViewGroupUtilsHoneycomb.offsetDescendantRect(parent, child, rect);
                }
            }

    private static class ViewGroupUtilsImplBase implements ViewGroupUtils.ViewGroupUtilsImpl {
                private ViewGroupUtilsImplBase() {

            }

        public void offsetDescendantRect(ViewGroup parent, View child, Rect rect) {
                        parent.offsetDescendantRectToMyCoords(child, rect);
                }
            }

    private interface ViewGroupUtilsImpl {
                void offsetDescendantRect(ViewGroup var1, View var2, Rect var3);
        }
    }
```

```java
class ViewGroupUtilsHoneycomb {
        private static final ThreadLocal<Matrix> sMatrix = new ThreadLocal<>();
    private static final ThreadLocal<RectF> sRectF = new ThreadLocal<>();
    private static final Matrix IDENTITY = new Matrix();

    public static void offsetDescendantRect(ViewGroup group, View child, Rect rect) {
                Matrix m = sMatrix.get();
            if (m == null) {
                    m = new Matrix();
                sMatrix.set(m);
            } else {
                    m.set(IDENTITY);
            }

        offsetDescendantMatrix(group, child, m);

        RectF rectF = sRectF.get();
                if (rectF == null) {
                        rectF = new RectF();
                }
                rectF.set(rect);
                m.mapRect(rectF);
                rect.set((int) (rectF.left + 0.5f), (int) (rectF.top + 0.5f),
                        (int) (rectF.right + 0.5f), (int) (rectF.bottom + 0.5f));
            }

    static void offsetDescendantMatrix(ViewParent target, View view, Matrix m) {
                final ViewParent parent = view.getParent();
            if (parent instanceof View && parent != target) {
                    final View vp = (View) parent;
                offsetDescendantMatrix(target, vp, m);
                m.preTranslate(-vp.getScrollX(), -vp.getScrollY());
            }

        m.preTranslate(view.getLeft(), view.getTop());

        if (!view.getMatrix().isIdentity()) {
                        m.preConcat(view.getMatrix());
                }
            }
        }
```


# Passing Data Through App

Assume we have a simple POJO which contains multiple information

```java
public class POJO
{
        public int integer;
    public String string;
    public Boolean bool;
    public Float f;

    public POJO(int integer, String string, Boolean bool, Float f)
        {
                this.integer = integer;
            this.string = string;
            this.bool = bool;
            this.f = f;
        }
    }
```

## 1. Normal intent

To pass data via an intent, use `putExtra()` in the intent.

Passing single variable to an activity, use this method.

```java
Intent intent = new Intent(context, MyActivity.class);
intent.putExtra("keyName","value");
intent.putExtra("keyName2", 2);
startActivity(intent);
```

To retrieve this data in the next Activity,

```java
String data = getIntent().getExtras().getString("keyName");
int data2 = getIntent().getExtras().getInt("keyName2", 0);
```

Alternatively, with null checking technique (recommended):

```java
Bundle extras = getIntent().getExtras();
if (extras != null) {
        String value = extras.getString("keyName");
    int data2 = extras.getInt("keyName2", 0);
}
```

## 2. Using Serializable

**Serializable** is a standard Java interface. You simply mark a class Serializable by implementing the interface, and Java will automatically serialize it in certain situations <sup>1</sup>.

Serializable - close to zero boilerplate, but it is the slowest approach and also requires hard-coded strings when pulling values out the intent (non-strongly typed).<sup>2</sup>

#### Make our POJO class implements Serializable

```java
public class POJO implements Serializable
{
        public int integer;
    public String string;
    public Boolean bool;
    public Float f;

    public POJO(int integer, String string, Boolean bool, Float f)
        {
                this.integer = integer;
            this.string = string;
            this.bool = bool;
            this.f = f;
        }
    }
```

#### Sending our POJO to another activity

```java
Intent intent = new Intent(context, MyActivity.class);
intent.putExtra("myPojoKey", new POJO(1, "2", true, 3.0f));
startActivity(intent);
```

#### Receiving our POJO intent

```java
Bundle extras = getIntent().getExtras();
if (extras != null) {
        POJO pojoObject = extras.getSerializableExtra("myPojoKey");
}
```

## 3. Using Parcelable

**Parcelable** is an Android specific interface where you implement the serialization yourself. It was created to be far more efficient that Serializable, and to get around some problems with the default Java serialization scheme. <sup>1</sup>

Parcelable - fast and Android standard, but it has lots of boilerplate code and requires hard-coded strings for reference when pulling values out the intent (non-strongly typed). <sup>2</sup>

#### Make our POJO implements Parcelable

**Protip:** In Android Studio IDE, this class can easily created by generating the boilerplate code.

```java
public class POJO implements Parcelable {
        public int integer;
    public String string;
    public Boolean bool;
    public Float f;

    public POJO(int integer, String string, Boolean bool, Float f) {
                this.integer = integer;
            this.string = string;
            this.bool = bool;
            this.f = f;
        }

    //used to deflate the POJO
        //before sending to destination activity
        @Override
        public void writeToParcel(Parcel dest, int flags) {
                dest.writeStringArray(new String[] {
                    String.valueOf(this.integer),
                this.string,
                String.valueOf(this.bool),
                String.valueOf(this.f)
            });
        }

    //used to inflate the POJO once it has
        //reached its destination activity
        public POJO(Parcel in) {
                String[] data = new String[4];
            in.readStringArray(data);
            this.integer = Integer.parseInt(data[0]);
            this.string = data[1];
            this.bool = Boolean.parseBoolean(data[2]);
            this.f = Float.parseFloat(data[3]);
        }

    //method on the interface
        @Override
        public int describeContents() {
                return 0;
        }

    //More boilerplate
        //Failure to add this results in the following exception
        //"android.os.BadParcelableException: Parcelable protocol 
        //requires a Parcelable.Creator object called  CREATOR on class"
        public static final Parcelable.Creator CREATOR = new Parcelable.Creator() {
                public POJO createFromParcel(Parcel in) {
                    return new POJO(in); 
            }

        public POJO[] newArray(int size) {
                        return new POJO[size];
                }
            };
        }
    ```

#### Sending our POJO to another activity

Similarly,

```java
Intent intent = new Intent(context, MyActivity.class);
intent.putExtra("myPojoKey", new POJO(1, "2", true, 3.0f));
startActivity(intent);
```

#### Receiving our POJO intent

```java
Bundle extras = getIntent().getExtras();
if (extras != null) {
        POJO pojoObject = extras.getParcelableExtra("myPojoKey"); 
}
```

## 4. Using EventBus

Event Bus - zero boilerplate, fastest approach, and does not require hard-coded strings, but it does require an additional dependency (although usually lightweight, ~40 KB). <sup>2</sup>

More detailed explanation: http://www.andreas-schrade.de/2015/11/28/android-how-to-use-the-greenrobot-eventbus/

#### Creating our bus

Simply call this line of code in anywhere to receive EventBus instance.

```java
EventBus myEventBus = EventBus.getDefault();
```

#### Creating our event

Guess what, our POJO is our event.

#### Creating our sender

Sending data on the bus is as easy as fuck.

```java
EventBus.getDefault().post(new POJO(1, "2", true, 3.0f));
```

#### Creating our subscriber (retriever)

**1. Register the bus**

Register inside `onCreate()`, `onStart()`, or `onResume()`.

```java
EventBus.getDefault().register(this); // this == your class instance
```

**2. Unregister the bus**

Read on why we need to unregister an instance: http://stackoverflow.com/a/31931742/996701

Unregister should happen inside `onDestroy()`, `onPause()` or `onStop()`.

```java
EventBus.getDefault().unregister(this);
```

**3. Retrieving the event**

```java
// This method will be called when a POJO event is posted
public void onEvent(HelloWorldEvent event){
      // your implementation
  processEvent(event);
}
```

#### Protip

Sticky events: http://greenrobot.org/eventbus/documentation/configuration/sticky-events/

---

#### Resources
1. http://stackoverflow.com/a/3323554/996701
2. http://stackoverflow.com/a/20056245/996701

# Basic ViewPager With Tabs

#### XML layout

```java
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/coordinatorLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <android.support.design.widget.AppBarLayout
            android:id="@+id/appbarLayout"
            android:layout_width="match_parent"
            android:layout_height="wrap_content">

        <include
                    android:id="@+id/toolbar"
                    layout="@layout/toolbar" />

        <android.support.design.widget.TabLayout
                    android:id="@+id/viewpagertab"
                    android:layout_width="match_parent"
                    android:layout_height="@dimen/tab_height"
                    android:background="@color/color_primary"
                    app:tabMode="fixed"
                    app:tabGravity="fill"
                    style="@style/CustomTabLayout" />

    </android.support.design.widget.AppBarLayout>

    <android.support.v4.view.ViewPager
            android:id="@+id/viewpager"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:layout_behavior="@string/appbar_scrolling_view_behavior" />

</android.support.design.widget.CoordinatorLayout>
```

#### Custom styling

```xml
<style name="CustomTabLayout" parent="Widget.Design.TabLayout">
    <item name="tabIndicatorColor">@color/white</item>
    <item name="tabIndicatorHeight">2dp</item>
    <item name="tabPaddingStart">12dp</item>
    <item name="tabPaddingEnd">12dp</item>
    <item name="tabBackground">?attr/selectableItemBackground</item>
    <item name="tabSelectedTextColor">@color/white</item>
    <item name="tabTextAppearance">@style/CustomTabTextAppearance</item>
</style>
```

#### Java

```java
public class MyViewPagerActivity extends AppCompatActivity {
    
        @BindView(R.id.toolbar)
            Toolbar toolbar;
            @BindView(R.id.viewpager)
            ViewPager viewPager;
            @BindView(R.id.viewpagertab)
            TabLayout viewPagerTab;

    @Override
        protected void onCreate(Bundle savedInstanceState) {
                ...
            initToolbar();
            initViewPager();
        }

    private void initToolbar() {
                if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
                    toolbar.setElevation(0);
            }
            setSupportActionBar(toolbar);
            getSupportActionBar().setTitle("Toolbar title");
            getSupportActionBar().setDisplayHomeAsUpEnabled(true);
            getSupportActionBar().setHomeButtonEnabled(false);
        }

    private void initViewPager() {
                MyTabAdapter adapter = new MyTabAdapter(getSupportFragmentManager(), this);
            adapter.addFragment(new MyFragment1());
            adapter.addFragment(new MyFragment2());
            adapter.addFragment(new MyFragment3());

        viewPager.setAdapter(adapter);
                viewPager.setOffscreenPageLimit(2); // Set the number of pages that should be retained to either side of the current page in the view hierarchy in an idle state. 
                viewPager.addOnPageChangeListener(new ViewPager.OnPageChangeListener() {
                        @Override
                    public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {

                    }

            @Override
                        public void onPageSelected(int position) {
                                // do something when a page is visible
                            // example: hiding a fab button
                        }

            @Override
                        public void onPageScrollStateChanged(int state) {

                        }
                    });
                    viewPagerTab.setupWithViewPager(viewPager);
                    for (int i = 0; i < viewPagerTab.getTabCount(); i++) {
                            TabLayout.Tab tab = viewPagerTab.getTabAt(i);
                        if (tab != null) {
                                tab.setCustomView(adapter.getTabView(i));
                        }
                    }
                }
            } 
```

#### Adapter

```java
public class MyTabAdapter extends FragmentPagerAdapter {
    
        private final List<Fragment> fragmentList = new ArrayList<>();
            private Context context;
            private String count = "";

    public MyTabAdapter(FragmentManager fm, Context context) {
                super(fm);
            this.context = context;
        }

    public void addFragment(Fragment fragment) {
                fragmentList.add(fragment);
        }

    @Override
        public Fragment getItem(int position) {
                return fragmentList.get(position);
        }

    @Override
        public CharSequence getPageTitle(int position) {
                return super.getPageTitle(position);
        }

    @Override
        public int getCount() {
                return fragmentList.size();
        }

    public void updateCount(String count) {
                this.count = count;
        }

    public View getTabView(int position) {
    
            // with custom tab layout - with counter badge
                    // invoked from parent Activity
                    // comment this whole section to use default layout

        View v = LayoutInflater.from(context).inflate(R.layout.custom_tab_impl, null);
                TextView text = (TextView) v.findBindView(R.id.custom_tab_text);
                TextView counter = (TextView) v.findBindView(R.id.custom_tab_counter);
                ImageView icon = (ImageView) v.findBindView(R.id.custom_tab_icon);

        switch (position) {
                        case 0:
                        text.setText(R.string.list_details_tabs_1);
                        counter.setVisibility(View.GONE);
                        icon.setVisibility(View.GONE);
                        break;
                   case 1:
                       text.setText(R.string.list_details_tabs_2);
                       counter.setVisibility(View.GONE);
                       if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
                               icon.setImageDrawable(icon.getContext().getDrawable(R.drawable.icon_camera_snp));
                       } else {
                               icon.setImageDrawable(icon.getContext().getResources().getDrawable(R.drawable.icon_camera_snp));
                       }
                       break;
                    case 1:
                        counter.setVisibility(View.GONE);
                        text.setText(R.string.list_details_tabs_3);
                        if (!count.isEmpty()) {
                                counter.setText(count);
                        } else {
                                counter.setVisibility(View.GONE);
                        }
                        icon.setVisibility(View.GONE);
                        break;
                    default:
                        throw new IllegalStateException("Invalid position: " + position);
                }

        return v;
            }
        }

```

### Modifications

#### 1. Lock swiping

Use this ViewPager instead of the native one.

```java
public class NonSwipingViewPager extends ViewPager {
        public NonSwipingViewPager(Context context) {
            super(context);
    }

    public NonSwipingViewPager(Context context, AttributeSet attrs) {
                super(context, attrs);
        }

    @Override
        public boolean onInterceptTouchEvent(MotionEvent ev) {
                return false;
        }

    @Override
        public boolean onTouchEvent(MotionEvent ev) {
                return false;
        }
    }

```

Implementation in xml:

```xml
<com.aemxn.com.ui.NonSwipingViewPager
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:layout_behavior="@string/appbar_scrolling_view_behavior" />
```

---

# Basic ViewPager Using PagerAdapter

A simpler version of viewpager

#### XML Layout

```xml
<android.support.v4.view.ViewPager
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="330dp"
    android:layout_gravity="bottom"
    android:padding="30dp"
    android:clipToPadding="false"
    android:overScrollMode="never"/>
```

#### Java

```java
    @BindView(R.id.viewpager)
    ViewPager viewPager;

    @Override
        public void onViewCreated(View view, @Nullable Bundle savedInstanceState) {
                super.onViewCreated(view, savedInstanceState);

        MyPagerAdapter pagerAdapter = new MyPagerAdapter();
                viewPager.setAdapter(pagerAdapter);
                pagerAdapter.add(data);
            }
    ```

#### Adapter

```java
public class MyPagerAdapter extends PagerAdapter {
    
        private List<MyObject> dataList;

    public LatestQuotePagerAdapter() {
                dataList = new ArrayList<>();
        }

    public void add(List<MyObject> dataList) {
                this.dataList = dataList;
            notifyDataSetChanged();
        }

    @Override
        public int getCount() {
                return dataList.size();
        }

    @Override
        public void destroyItem(ViewGroup container, int position, Object object) {
                container.removeView((View) object);
        }

    @Override
        public Object instantiateItem(ViewGroup container, final int position) {
                View view = View.inflate(container.getContext(), R.layout.row_item, null);

        container.addView(view);

        return view;
            }

    @Override
        public boolean isViewFromObject(View view, Object object) {
                return view == object;
        }
    }

```

---

# Using Fragment in an Activity

### 1. FragmentManager add/replace FrameLayout ID

Read on add or replace: http://stackoverflow.com/questions/18634207/difference-between-add-replace-and-addtobackstack

```java
MyFragment myFragment = new MyFragment();
getSupportFragmentManager()
    .beginTransaction()
    .replace(R.id.frame_container, myFragment)
    .commit();
```

### 2. Using newInstance (passing data)

#### Activity

```java
Fragment myFragment = MyFragment.newInstance("passing title");
getSupportFragmentManager()
    .beginTransaction()
    .replace(R.id.frame_container, myFragment)
    .commit();
```

#### Fragment

```java
    public static MyFragment newInstance(String title) {
            MyFragment f = new MyFragment();
        Bundle args = new Bundle();
        args.putString("title", title);
        f.setArguments(args);
        return (f);
    }
```

#### Retrieving the Bundle args

```java
@Override
public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        View rootView = inflater.inflate(R.layout.fragment_my, container, false);

    Bundle b = getArguments();
        if (b != null) {
                String s = b.getString("title");
        }

    // alternatively,
        // String s = getArguments().getString("title");

    return rootView;
    }
```

# Building a Preference Page

### 1. Activity

#### Layout

```xml
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <include
            android:id="@+id/toolbar"
            layout="@layout/toolbar" />

    <FrameLayout
            android:id="@+id/content_frame"
            android:layout_width="match_parent"
            android:layout_height="wrap_content" />

</LinearLayout>
```

#### Java

```java
@EActivity
public class SettingsActivity extends AppCompatActivity {
    
        @ViewById(R.id.toolbar)
            Toolbar toolbar;

    @Override
        protected void onPostCreate(Bundle savedInstanceState) {
                super.onPostCreate(savedInstanceState);
            setContentView(R.layout.activity_settings);

        toolbar.setTitle("Settings");
                setSupportActionBar(toolbar);

        if (savedInstanceState == null) {
                        getFragmentManager()
                            .beginTransaction()
                            .replace(R.id.content_frame, new SettingsFragment_())
                            .commit();
                }

        //set the back arrow in the toolbar
                getSupportActionBar().setDisplayHomeAsUpEnabled(true);
                getSupportActionBar().setHomeButtonEnabled(false);
            }

    @Override
        public boolean onOptionsItemSelected(MenuItem item) {
                switch (item.getItemId()) {
                    case android.R.id.home:
                    onBackPressed();
                    return true;
                default:
                    return false;
            }
        }

}
```

### 2. PreferenceFragment

#### Layout

```xml
<?xml version="1.0" encoding="utf-8"?>
<PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android">

    <PreferenceCategory android:title="Notifications">
            <CheckBoxPreference
                android:defaultValue="true"
                android:key="@string/key_pref_notification"
                android:summary="Receive push notifications"
                android:title="Push notifications" />
        </PreferenceCategory>
        <PreferenceCategory android:title="Media">
            <CheckBoxPreference
                android:defaultValue="true"
                android:key="@string/key_pref_media_auto_download"
                android:summary="Download photos automatically"
                android:title="Media auto-download" />
        </PreferenceCategory>
        <PreferenceCategory android:title="About">
            <Preference
                android:key="@string/key_pref_faq"
                android:summary="Frequently Asked Questions"
                android:title="FAQs" />
            <Preference
                android:key="@string/key_pref_help"
                android:title="Help" />
            <Preference
                android:key="@string/key_pref_about"
                android:title="Version" />
        </PreferenceCategory>


</PreferenceScreen>
```

#### Java

```java
public class SettingsFragment extends PreferenceFragment {
    
        Preference prefAbout;
            CheckBoxPreference prefNotify;
            CheckBoxPreference autoDownload;

    @Override
        public void onCreate(Bundle savedInstanceState) {
                super.onCreate(savedInstanceState);
            addPreferencesFromResource(R.xml.settings);

        // initialize viewgroups
                prefNotify = (CheckBoxPreference) getPreferenceManager().findPreference("preference_key");
            }


    @Override
        public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
    
            prefNotify.setChecked(true);
                    prefNotify.setOnPreferenceChangeListener(new Preference.OnPreferenceChangeListener() {
                            @Override
                        public boolean onPreferenceChange(Preference preference, Object newValue) {
                                prefNotify.setChecked((boolean) newValue);
                            return false;
                        }
                    });

        autoDownload.setChecked(session.autoDownload().getOr(false));

        try {
                        PackageInfo pInfo = getActivity().getPackageManager().getPackageInfo(getActivity().getPackageName(), 0);
                    String vName = pInfo.versionName;
                    if (BuildConfig.DEBUG) {
                            prefAbout.setSummary(String.format(getResources().getString(R.string.about_version_name_debug), vName));
                    } else {
                            prefAbout.setSummary(String.format(getResources().getString(R.string.about_version_name_production), vName));
                    }
                } catch (PackageManager.NameNotFoundException e) {
                        e.printStackTrace();
                }
            }

}
```

# Long Running Task

More detailed explanation on [background tasks](https://blog.yipl.com.np/things-to-consider-before-running-background-tasks-e71f00d2ad3a#.78nmox8nu).

### AsyncTask

How it works: http://i.stack.imgur.com/I23KW.png

```java
public void onClick(View view) {
        new LongOperation().execute();
    break;
}

private class LongOperation extends AsyncTask<String, Void, String> {
    
        @Override
            protected void onPreExecute() {
                    // initializer
                // stuff to do before background task started
            }

    @Override
        protected String doInBackground(String... params) {
                // stuff to do in background
            return null;
        }

    @Override
        protected void onPostExecute(String result) {
                // stuff to do after task is finished
        }


    @Override
        protected void onProgressUpdate(Void... values) {}
    }
```

### Thread

Use for long running process. We seperate process in a thread outside from main (so that we don't touch UI).

```java
Thread t = new Thread() {
        @Override
    public void run(){
            doSomeWork();
        if(succeed){
                //we can't update the UI from here so we'll signal our handler and it will do it for us.
            handler.sendEmptyMessage(0);
        }else{
                handler.sendEmptyMessage(1);
        }
    }   
};
```

### Handler with delay

Used to communicate between the UI and Background thread.

```java
Handler handler = new Handler(){
        @Override
    public void handleMessage(Message msg){
            if(msg.what == 0){
                updateUI();
        }else{
                showErrorDialog();
        }
    }
};
```

To delay a process

```java
handler.postDelayed(new Runnable(){
        @Override
    public void run() {
            // process run with 1s delay
    }
}, 1000);
```

# Runtime Permission

The basic of runtime permission as of Android 6.0

Our plan is to make use of Camera and Storage permission. Declare these in the manifest file.

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.CAMERA" />
```

#### Required variable

```java
private static final int REQUEST_ALL_PERMISSION = 1;
```

#### Initializing the permission.

Notice in the if statements, we are using two permission at the same time (`CAMERA` and `READ\_EXTERNAL\_STORAGE`). This is called multiple permission. App will pop up a dialog with multiple permission prompt. Remove one of this to use single permission.

```java
@TargetApi(Build.VERSION_CODES.M)
private void initPermission() {
        if (ContextCompat.checkSelfPermission(this, Manifest.permission.CAMERA) != PackageManager.PERMISSION_GRANTED ||
            ContextCompat.checkSelfPermission(this, Manifest.permission.READ_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED) {
            if (shouldShowRequestPermissionRationale(Manifest.permission.CAMERA) ||
                shouldShowRequestPermissionRationale(Manifest.permission.READ_EXTERNAL_STORAGE)) {
                Toast.makeText(context, "Need permission please", Toast.LENGTH_SHORT).show();
        }
        requestPermissions(new String[]{Manifest.permission.CAMERA, Manifest.permission.READ_EXTERNAL_STORAGE}, REQUEST_ALL_PERMISSION);
    } else {
            // do your normal stuff
    }
}
```

#### Receiving permission result

If using single permission, make sure to assign a static variable for that particular permission and append more `case` statements.  

```java
@Override
public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        switch (requestCode) {
            case REQUEST_ALL_PERMISSION:
            if (grantResults.length > 0 &&
                    grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                            // do your normal stuff
            } else {
                    // showing a dialog that directs user to app's detail setting
                AlertDialogUtils.appDetails(this, R.string.permission_appdetails_media);
            }
            return;
        default:
            super.onRequestPermissionsResult(requestCode, permissions, grantResults);
    }
}
```

# Optimizing Large Bitmaps

Often times, we face OOM error when loading a large bitmaps in our app.

This is mainly because images come in all shapes and sizes. In many cases they are larger than required for a typical application user interface (UI). For example, the system Gallery application displays photos taken using your Android devices's camera which are typically much higher resolution than the screen density of your device.

Given that you are working with limited memory, ideally you only want to load a lower resolution version in memory. The lower resolution version should match the size of the UI component that displays it. An image with a higher resolution does not provide any visible benefit, but still takes up precious memory and incurs additional performance overhead due to additional on the fly scaling.

Loading here literally means anything that requires your app to load Bitmap from any sources, be it setting it to a view or loading from storage. In this example, we are demonstrating with `decodeFile()` in which we are converting a File to a Bitmap.

```java
BitmapFactory.Options options = new BitmapFactory.Options();
options.inJustDecodeBounds = false;
options.inPreferredConfig = Bitmap.Config.RGB_565;
options.inDither = true;
options.inSampleSize = 3;

Bitmap cropImage = BitmapFactory.decodeFile(file.getPath(), options);
```

More info of the options here: https://developer.android.com/reference/android/graphics/BitmapFactory.Options.html

Most importantly, the `inSampleSize` plays an important part in this snippet. As per docs,

> If set to a value > 1, requests the decoder to subsample the original image, **returning a smaller image to save memory**. The sample size is the number of pixels in either dimension that correspond to a single pixel in the decoded bitmap.
> For example, inSampleSize == 4 returns an image that is 1/4 the width/height of the original, and 1/16 the number of pixels. Any value <= 1 is treated the same as 1. Note: the decoder uses a final value based on powers of 2, any other value will be rounded down to the nearest power of 2.

# Displaying Notifications

#### Notification with a button and a click listener

```java
Intent openIntent = new Intent(context, DrawerActivity.class);
openIntent.setFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP | Intent.FLAG_ACTIVITY_SINGLE_TOP);
openIntent.putExtra(Constants.FROM_NOTIFICATION, true);
PendingIntent openPendingIntent = PendingIntent.getActivity(context, 0, openIntent, PendingIntent.FLAG_UPDATE_CURRENT);

Intent saveIntent = new Intent(KEY_SAVE);
saveIntent.putExtra(QuotesPreference.KEY_QUOTE, quote);
saveIntent.putExtra(QuotesPreference.KEY_AUTHOR, author);
saveIntent.putExtra(QuotesPreference.KEY_QUOTE_TYPE, quote_type);
saveIntent.putExtra(QuotesPreference.KEY_QUOTE_ID, quote_id);
PendingIntent savePendingIntent = PendingIntent.getBroadcast(context, 0, saveIntent, PendingIntent.FLAG_UPDATE_CURRENT);

NotificationCompat.Builder mBuilder = new NotificationCompat.Builder(context)
        .setLargeIcon(bm)
        .setSmallIcon(R.drawable.noti_small_icon)
        .setStyle(new NotificationCompat.BigTextStyle()
                .bigText(notification_body))
        .setAutoCancel(true)
        .setContentTitle(title)
        .setContentText(notification_body)
        .setContentIntent(openPendingIntent)
        .setDefaults(Notification.DEFAULT_VIBRATE | Notification.DEFAULT_LIGHTS)
        .setSound(Settings.System.DEFAULT_NOTIFICATION_URI)
        .addAction(R.mipmap.ic_save, "Save", savePendingIntent);
        NotificationManager mNotificationManager = (NotificationManager) context
        .getSystemService(Context.NOTIFICATION_SERVICE);
        mNotificationManager.notify((int) timeInMillis, mBuilder.build());
```
# Android Logging Utility

Below is a snippet code for logging your application in logcat.

**Usage:**

Simply create a class called `L.java` and use it in your code.

```java
public final class L {
    
        private static final String LOG_TAG = "My_Logcat";

    private static final boolean DEBUG = true;

    public static void e(String message, Throwable cause) {
                if (DEBUG) {
                    Log.e(LOG_TAG, "[" + message + "]", cause);
            }
        }

    public static void e(String msg) {
                if (DEBUG) {
                    Throwable t = new Throwable();
                StackTraceElement[] elements = t.getStackTrace();

            String callerClassName = elements[1].getFileName();
                        Log.e(LOG_TAG, "[" + callerClassName + "] " + msg);
                    }
                }

    public static void w(String message, Throwable cause) {
                if (DEBUG) {
                    Log.w(LOG_TAG, "[" + message + "]", cause);
            }
        }

    public static void w(String msg) {
                if (DEBUG) {
                    Throwable t = new Throwable();
                StackTraceElement[] elements = t.getStackTrace();

            String callerClassName = elements[1].getFileName();
                        Log.w(LOG_TAG, "[" + callerClassName + "] " + msg);
                    }
                }

    public static void j(String title, Object msg) {
                if (DEBUG) {
                    Throwable t = new Throwable();
                StackTraceElement[] elements = t.getStackTrace();

            String callerClassName = elements[1].getFileName();
                        Log.w(LOG_TAG, "[" + callerClassName + "] " + title + ": " + new Gson().toJson(msg));
                    }
                }

    public static void i(String message, Throwable cause) {
                if (DEBUG) {
                    Log.i(LOG_TAG, "[" + message + "]", cause);
            }
        }

    public static void i(String msg) {
                if (DEBUG) {
                    Throwable t = new Throwable();
                StackTraceElement[] elements = t.getStackTrace();

            String callerClassName = elements[1].getFileName();
                        Log.i(LOG_TAG, "[" + callerClassName + "] " + msg);
                    }
                }

    public static void i(String msg, Object... args) {
                if (DEBUG) {
                    Throwable t = new Throwable();
                StackTraceElement[] elements = t.getStackTrace();

            String callerClassName = elements[1].getFileName();
                        Log.i(LOG_TAG, "[" + callerClassName + "] " + String.format(msg, args));
                    }
                }

    public static void d(String msg, Throwable cause) {
                if (DEBUG) {
                    Log.d(LOG_TAG, msg, cause);
            }
        }

    public static void d(String msg) {
                if (DEBUG) {
                    Throwable t = new Throwable();
                StackTraceElement[] elements = t.getStackTrace();

            String callerClassName = elements[1].getFileName();
                        Log.d(LOG_TAG, "[" + callerClassName + "] " + msg);
                    }
                }

    public static void v(String msg, Throwable cause) {
                if (DEBUG) {
                    Log.v(LOG_TAG, msg, cause);
            }
        }

    public static void v(String msg) {
                if (DEBUG) {
                    Throwable t = new Throwable();
                StackTraceElement[] elements = t.getStackTrace();

            String callerClassName = elements[1].getFileName();
                        Log.v(LOG_TAG, "[" + callerClassName + "] " + msg);
                    }
                }

    public static void v(String format, Object... args) {
                if (DEBUG) {
                    Throwable t = new Throwable();
                StackTraceElement[] elements = t.getStackTrace();

            String callerClassName = elements[1].getFileName();
                        Log.v(LOG_TAG, "[" + callerClassName + "] " + String.format(format, args));
                    }
                }

    /**
         * Logging on steroids
         */
        public static void tag(String tag, String msg) {
                if (DEBUG) {
                    Throwable t = new Throwable();
                StackTraceElement[] elements = t.getStackTrace();

            String callerClassName = elements[1].getFileName();
                        Log.wtf(LOG_TAG,
                                "<" + callerClassName + ">\\
            " +
                                        "*******************************\\
            " +
                                        "[" + tag + "]\\
            " +
                                        msg);
                    }
                }
            }
```

### Toasting

```java
public final class T {
    
        /**
             * Show custom toast
             */
            public static void show(Context context, String content) {
                    View v = View.inflate(context.getApplicationContext(), R.layout.layout_toast, null);
                ((TextView)v.findViewById(R.id.text)).setText(content);
                Toast t = new Toast(context.getApplicationContext());
                t.setView(v);
                t.setGravity(Gravity.TOP,0,0);
                t.show();
            }

    /**
         * Show long duration toast
         */
        public static void showL(Context context, String content) {
                Toast.makeText(context, content, Toast.LENGTH_LONG).show();
        }

    /**
         * Show short duration toast
         */
        public static void showS(Context context, String content) {
                Toast.makeText(context, content, Toast.LENGTH_SHORT).show();
        }

    /**
         * Showing network error toast
         */
        public static void showNetErr(Context context) {
                Toast.makeText(context, "404 Internet Not Found", Toast.LENGTH_SHORT).show();
        }

}
```

### Snackbar

It's pretty straightforward you can even create a class for managing Snackbars.

```java
public final class SB {
    
        /**
             * Show long duration Snackbar
             */
            public static void showL(View view, String content) {
                     Snackbar.make(view, content, Snackbar.LENGTH_LONG).show();
            }

    /**
         * Show short duration Snackbar
         */
        public static void showS(View view, String content) {
                 Snackbar.make(view, content, Snackbar.LENGTH_SHORT).show();
        }

}
```