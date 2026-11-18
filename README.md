# 🐶 Dogglers (PMDM · UD1)

An Android app in **Kotlin** that shows a list of dogs as cards in three layouts — vertical, horizontal and grid — using `RecyclerView`. Based on Google's *Android Basics in Kotlin* starter project, completed as an exercise for Unit 1 of *Programación Multimedia y Dispositivos Móviles* (PMDM).

## 📁 Project Structure

```
app/src/main/java/com/example/dogglers/
├── MainActivity.kt              → Three buttons to open each list layout
├── VerticalListActivity.kt      → RecyclerView with a vertical LinearLayoutManager
├── HorizontalListActivity.kt    → RecyclerView with a horizontal LinearLayoutManager
├── GridListActivity.kt          → RecyclerView with a GridLayoutManager
├── adapter/DogCardAdapter.kt    → The adapter (the part completed in this exercise)
├── data/DataSource.kt           → Hard-coded list of dogs
├── model/Dog.kt                 → Data class: image, name, age, hobbies
└── const/Layout.kt              → Constants for VERTICAL / HORIZONTAL / GRID
app/src/androidTest/             → Provided UI tests for each layout (not to be modified)
```

## 🚀 How It Works

One adapter serves the three screens. It receives the layout type and inflates a different item layout for the grid:

```kotlin
val layoutId = if (layout == Layout.GRID) R.layout.grid_list_item
               else R.layout.vertical_horizontal_list_item
```

`onBindViewHolder` fills each card from `DataSource.dogs`, using string resources with placeholders for age and hobbies:

```kotlin
holder.imageView.setImageResource(dog.imageResourceId)
holder.nameTextView.text = dog.name
holder.ageTextView.text = resources?.getString(R.string.dog_age, dog.age)
```

The provided instrumented tests check that each list shows the right data in the right layout.

## 🧠 Key Concepts

- **RecyclerView** — `Adapter`, `ViewHolder`, `onCreateViewHolder`, `onBindViewHolder`
- **Layout managers** — vertical / horizontal `LinearLayoutManager` and `GridLayoutManager`
- **Reusing one adapter** for several layouts
- **String resources with arguments** — `getString(R.string.x, value)`
- **Instrumented UI tests** — Espresso tests in `androidTest`

## 🛠️ How To Run

Open the project in **Android Studio** and run it on an emulator or device. To run the provided tests:

```bash
./gradlew connectedAndroidTest
```
