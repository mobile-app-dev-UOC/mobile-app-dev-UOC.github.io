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
- *StaggeredGridLayoutManager*: Displays items in a staggered grid. Each item can have a different height and the size of rows is not adjusted using blank space. In fact, the concept of row disappears.

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

Here we indicate that two items are the same if they have the same reference. We also indicate that one item has the same content as another if the `id` and `name` match.

The update process would be as follows:

a) For this method to work,  the first time, in the `onCreate` of the activity, we assign a copy of the dataSource list to the adapter:

```kotlin
var new_list:MutableList<Item> = ArrayList()
itemsListViewModel.dataSource.ItemsLiveData.value?.let { it1 -> new_list.addAll(it1) }

itemsAdapter.submitList(new_list)
```

b) We modify the DataSource to perform the test.

```kotlin
val data:DataSource = DataSource.getDataSource(this.resources)
var item:Item = data.getItemForId(3)!!
var item2:Item = item.copy()
item2.name = "updated"
data.updateItem(item,item2)
data.addItem(Item(333,"New flower",R.drawable.rose,"new item"))
data.CommitChanges()
```

In this code, we make an update and an insertion.

Please note that, in order to modify an item, we have to create a copy. If we do not do so, the item would maintain the same reference and the Diff would indicate that it has not been modified. Once all the modifications have been applied, we will execute the DataSource `CommitChanges` method so that the viewModel is informed of the existence of changes.

**Important**: We must notify the viewModel only when **all** changes have already been made. If we notify after each change instead, the second change may be lost: when the notification of the second change arrived, the two lists would already be the same and therefore the DiffUtil callback would not notice a difference.

The method `CommitChanges` indicates that changes have occurred using `ItemsLiveData.postValue`.

```kotlin
fun CommitChanges()
{
   val currentList = ItemsLiveData.value
   val updatedList = currentList!!.toMutableList()
   ItemsLiveData.postValue(updatedList)
}
```

c) This method will cause the model observer to run in the activity:

```kotlin
itemsListViewModel.itemsLiveData.observe(this, {
           it?.let {
               itemsAdapter.submitList(it)
           }
       })
```

The observer calls `submitList` with the list it receives as a parameter. The model will be updated after the call is executed.

2. The second mechanism is to share the model with the adapter as well. 

a) To do this in the `onCreate` callback of the activity, we assign the exact same list that is contained in the model. 

```kotlin
itemsAdapter.submitList(DataSource.getDataSource(this.resources)
	.getItemList().value)
```

b) We make changes directly to the list shared by dataSource and Adapter.

```kotlin
Val data: DataSource = DataSource.getDataSource(this.resources)
var item:Item? = data.getItemForId(3)
item?.name = “updated”

itemsListViewModel.dataSource.ItemsLiveData.value!!.add(6,Item(333,"New flower",R.drawable.rose,"new item"))

itemsListViewModel.dataSource.ItemsLiveData.postValue(itemsListViewModel.dataSource.ItemsLiveData.value)
```

Likewise, at the end we invoke the `postValue` method so that the observer of the activity is triggered.

```kotlin
itemsListViewModel.itemsLiveData.observe(this, {
   it?.let {   
       itemsAdapter.notifyItemChanged(2)
       itemsAdapter.notifyItemInserted(6)
    }
})
```

Then we manually indicate, by means of `notifyItemChanged` and `notifyItemInserted`, the changes we have made. 

In the example above, our code is slightly forced: we should have implemented a change queue in the dataSource and pass that queue to the observer of the activity as a parameter.

To avoid having to implement this queue, we can perform: 
```kotlin
itemsAdapter.notifyDataSetChanged()
```

This method refreshes the entire list but it is expensive in terms of CPU and memory usage if we have very long lists.

The first approach (`notifyItemChanged` and `notifyItemInserted`) is the easiest to code and is almost optimal thanks to the DiffUtil callback. Meanwhile, the second approach (`notifyDataSetChanged`)is closer to optimal but requires that we implement an update queue pattern so that in the observer of the model we know what notifications need to be made.


