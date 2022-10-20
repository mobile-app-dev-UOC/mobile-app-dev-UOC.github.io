---
layout: default
title: 5.7. Adapters, Models and RecyclerView
parent: 5. User interfaces
nav_order: 7
---

# 5.7. Adapters, Models and RecyclerView

## RecyclerView

The RecyclerView is the successor to ListView and GridView and provides much more flexibility and performance. This component can display items differently depending on the LayoutManager assigned to it. 

> ![Example of a RecyclerView.](/images/05/recycler-view.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Example of a RecyclerView.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

LayoutManager Types:

- *LinearLayoutManager*: Displays items in a list with vertical or horizontal scrolling.
- *GridLayoutManager*: Displays items in a grid.
- *StaggeredGridLayoutManager: Displays items in a staggered grid. Each item can have a different height and the size of rows is not adjusted using blank space. In fact, the concept of row disappears.

There are two types of scenarios:
1.	We want to load a RecyclerView with contents that once uploaded will be static.
2.	Data can be modified in any way once it have already been uploaded to RecyclerView.

Let us study the second scenario because it includes the first.

The elements we have are:

- *DataSource*: the source of the data.
- *viewModel*: The viewModel that will allow us to observe changes in the data source.
- *Adapter*: The adapter is an intermediary between the data source and the view.
- *RecyclerView*: The class that will display the data.

These items communicate among them as shown in the following sequence diagram:

> ![Sequence diagram depiciting the communication between the elements in a RecyclerView.](/images/05/sequence-diagram.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Sequence diagram showing the communication between the elements in a RecyclerView.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Let us inspect the diagram above using a simple example.

We define our DataSource as a class that provides access to our content. You could access remote content and then store it in an ItemList. 

```kotlin
class DataSource(resources: Resources) {
   public val initialItemList = ItemList(resources)
   public val ItemsLiveData = MutableLiveData(initialItemList)
}
```

The ItemList class is defined as:

```kotlin
fun ItemList(resources: Resources): MutableList<Item>
```

The ItemList class is defined as:

```kotlin
fun ItemList(resources: Resources): MutableList<Item>
```

An item is defined as:
```kotlin
data class Item(
   val id: Long,
   var name: String,
   @DrawableRes
   val image: Int?,
   val description: String
)
```

In our main activity we define a viewModel connected to the same data source.

```kotlin
private val itemsListViewModel by viewModels<ItemsListViewModel> {
   ItemsListViewModelFactory(this)
}
```

The data source is created using a [Singleton](https://en.wikipedia.org/wiki/Singleton_pattern) design pattern. That is, there is a single instance that is shared throughout the activity. Our viewModel monitors changes to the `ItemsLiveData` property of the data source.

Then, we register the activity to receive changes to that viewModel.

```kotlin
itemsListViewModel.itemsLiveData.observe(this, {
   it?.let {

    // Update Code

    }
})
```

Now we have created an Adapter type class. In our case, we inherit from ListAdapter:

```kotlin
class ItemsAdapter(private val onClick: (Item) -> Unit) :
   ListAdapter<Item, ItemsAdapter.ItemViewHolder>(ItemDiffCallback) {
}
```

In the `onCreate` callback of our Activity, we create the RecyclerView inside the activity and assign the Adapter to it.

```kotlin
val itemsAdapter = ItemsAdapter { item -> adapterOnClick(item) }
val recyclerView: RecyclerView = binding.recyclerList
recyclerView.adapter = itemsAdapter
```

Finally, we decide that we want it to be drawn as a vertical list and therefore we select a LinearLayoutManager:

```kotlin
recyclerView.setLayoutManager(LinearLayoutManager(this));
```

---

## Adapter

Let us take a closer look at the role of our adapter. We must overwrite three basic methods:

```kotlin
override fun getItemCount(): Int
```

This method should return the number of items in the list.

```kotlin
override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ItemViewHolder
```

This method should return the view that represents a row. In this case, we return a class that inherits from `RecyclerView.ViewHolder` and implements the `click` event and a way to load an item’s data into that view.

```kotlin
class ItemViewHolder(itemView: View, val onClick: (Item) -> Unit) :
   RecyclerView.ViewHolder(itemView) {
   private val itemTextView: TextView = itemView.findViewById(R.id.item_text)
   private val itemImageView: ImageView = itemView.findViewById(R.id.item_image)
   private var currentItem: Item? = null

   init {
       itemView.setOnClickListener {
           currentItem?.let {
               onClick(it)
           }
       }
   }


   /* Bind item name and image. */
   fun bind(item: Item) {
       currentItem = item

       itemTextView.text = item.name
       if (item.image != null) {
           itemImageView.setImageResource(item.image)
       } else {
           itemImageView.setImageResource(R.drawable.rose)
       }
   }
}
```

We also define a method that allows an ItemViewHolder to be connected to an instance of the Item class:

```kotlin
override fun onBindViewHolder(holder: ItemViewHolder, position: Int) {
   val item = getItem(position)
   holder.bind(item)
}
```

On the other hand, the Adapter builder is instantiated with an implementation of `DiffUtil.ItemCallback`. The purpose of this callback is being able to decide if two items are the same. It is used to compare the list inside the adapter and the list it will receive when the method `submitList` is invoked.

Once all this is defined in the `onCreate` callback of the main Activity, we can use submitList to make the first assignment in the list.

```kotlin
itemsAdapter.submitList(DataSource.getDataSource(this.resources)
	.getItemList().value)
```

With al the previous code, the view will be updated.

---

## Updating the RecyclerView

There are two mechanisms to refresh the RecyclerView.

1. We can reassign the list of items each time a change occurs in the data source, given that these modifications are being monitored by the viewModel. We use `submitList` after every change to reassign the list.

This operation is not as expensive in terms of time and space as it would seem at first, thanks to the `DiffUtil.ItemCallback` callback we have defined. This callbak will allow the adapter to determine which items have been modified, added, deleted, or moved.

Consider a DiffUtil callback defined as follows:

```kotlin
object ItemDiffCallback : DiffUtil.ItemCallback<Item>() {
   override fun areItemsTheSame(oldItem: Item, newItem: Item): Boolean {
       return oldItem == newItem
   }

   override fun areContentsTheSame(oldItem: Item, newItem: Item): Boolean {
       return oldItem.id == newItem.id && oldItem.name == newItem.name
   }
}
```


