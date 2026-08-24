# Practical-3: Implicit & Explicit Intent

## AIM

Create an Android application that demonstrates the use of **Implicit Intent** and **Explicit Intent**.

The application provides buttons to perform the following operations:

1. Make a call to a specific number
2. Open a specific URL
3. Open Call Log
4. Open Gallery
5. Set an Alarm
6. Open Camera
7. Open Login Activity

---

# Introduction

An **Intent** is a messaging object used by Android components to request an action from another component.

Intents are mainly classified into:

* **Implicit Intent**
* **Explicit Intent**

## Implicit Intent

An implicit intent does not specify the exact Android component that should handle the request. Instead, Android finds an appropriate application/component based on the action and data.

Examples:

* Opening a website
* Opening the Gallery
* Opening the Camera
* Opening Call Log
* Setting an Alarm

## Explicit Intent

An explicit intent specifies the exact component or Activity that should be launched.

Example:

* Opening `LoginActivity` from `MainActivity`

---

# Operations

## 1. Make Call to Specific Number

Use an Intent with the `tel:` URI scheme.

Example:

```kotlin
val intent = Intent(Intent.ACTION_CALL)
intent.data = Uri.parse("tel:1234567890")
startActivity(intent)
```

The application requires the appropriate phone permission in the manifest and must request runtime permission where required.

### Permission

```xml
<uses-permission android:name="android.permission.CALL_PHONE" />
```

---

# 2. Open Specific URL

Use `ACTION_VIEW` to open a URL in a browser.

```kotlin
val intent = Intent(
    Intent.ACTION_VIEW,
    Uri.parse("https://www.google.com")
)
startActivity(intent)
```

### Concepts

* `Intent.ACTION_VIEW`
* `Uri.parse()`
* `Intent.setData()`
* `startActivity()`

---

# 3. Open Call Log

Use an implicit intent to open the device's Call Log.

```kotlin
val intent = Intent(Intent.ACTION_VIEW)
intent.type = CallLog.Calls.CONTENT_TYPE
startActivity(intent)
```

The relevant Android content type is:

```text
CallLog.Calls.CONTENT_TYPE
```

Depending on the Android version and device, access to call-log information may require appropriate permissions.

---

# 4. Open Gallery

Use an implicit intent with an image MIME type.

```kotlin
val intent = Intent(Intent.ACTION_VIEW)
intent.type = "image/*"
startActivity(intent)
```

Another common approach for selecting an image is:

```kotlin
val intent = Intent(Intent.ACTION_GET_CONTENT)
intent.type = "image/*"
startActivity(intent)
```

### Important MIME Type

```text
image/*
```

---

# 5. Set Alarm

Use the Android Alarm Clock intent.

```kotlin
val intent = Intent(AlarmClock.ACTION_SET_ALARM).apply {
    putExtra(AlarmClock.EXTRA_MESSAGE, "Wake Up")
    putExtra(AlarmClock.EXTRA_HOUR, 7)
    putExtra(AlarmClock.EXTRA_MINUTES, 0)
}

startActivity(intent)
```

This launches an alarm application capable of handling the request.

---

# 6. Open Camera

Use an implicit intent with `ACTION_IMAGE_CAPTURE`.

```kotlin
val intent = Intent(MediaStore.ACTION_IMAGE_CAPTURE)
startActivity(intent)
```

For modern Android applications, the recommended approach is to use an appropriate Activity Result API contract, such as `ActivityResultContracts.TakePicture()` or `ActivityResultContracts.TakePicturePreview()` depending on the application's requirements.

Camera permission may be required when directly accessing the camera.

```xml
<uses-permission android:name="android.permission.CAMERA" />
```

---

# 7. Open Login Activity

This demonstrates an **Explicit Intent**.

Suppose the application contains:

```text
MainActivity
LoginActivity
```

The Login Activity can be opened using:

```kotlin
val intent = Intent(this, LoginActivity::class.java)
startActivity(intent)
```

Unlike an implicit intent, the destination Activity is explicitly specified.

---

# Required UI

The Main Activity should contain buttons for each operation.

Recommended buttons:

```text
Make Call
Open URL
Open Call Log
Open Gallery
Set Alarm
Open Camera
Open Login Activity
```

The UI can be implemented using:

* `ConstraintLayout`
* `CoordinatorLayout`
* `Button`

Example structure:

```xml
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <!-- Buttons for Intent operations -->

</androidx.constraintlayout.widget.ConstraintLayout>
```

---

# Intent Actions

The following Intent actions are demonstrated:

| Operation           | Intent / Action                             |
| ------------------- | ------------------------------------------- |
| Make Call           | `Intent.ACTION_CALL`                        |
| Open URL            | `Intent.ACTION_VIEW`                        |
| Open Call Log       | `Intent.ACTION_VIEW`                        |
| Open Gallery        | `Intent.ACTION_VIEW` / `ACTION_GET_CONTENT` |
| Set Alarm           | `AlarmClock.ACTION_SET_ALARM`               |
| Open Camera         | `MediaStore.ACTION_IMAGE_CAPTURE`           |
| Open Login Activity | Explicit `Intent`                           |

---

# Intent.setData()

`setData()` is used to specify the data that an Intent should operate on.

Example:

```kotlin
val intent = Intent(Intent.ACTION_VIEW)
intent.setData(Uri.parse("https://www.google.com"))
startActivity(intent)
```

For a telephone URI:

```kotlin
intent.setData(Uri.parse("tel:1234567890"))
```

---

# Intent.setType()

`setType()` specifies the MIME type of the data.

Example:

```kotlin
val intent = Intent(Intent.ACTION_VIEW)
intent.setType("image/*")
startActivity(intent)
```

The Gallery example uses:

```text
image/*
```

---

# Uri.parse()

`Uri.parse()` converts a URI string into a `Uri` object.

Example:

```kotlin
val uri = Uri.parse("https://www.google.com")
```

It can also be used with telephone numbers:

```kotlin
val uri = Uri.parse("tel:1234567890")
```

---

# Permissions

Some Intent operations require permissions.

## Manifest Permissions

Example:

```xml
<uses-permission android:name="android.permission.CALL_PHONE" />
<uses-permission android:name="android.permission.CAMERA" />
```

Permissions should only be requested when they are actually required by the application.

---

# Runtime Permission

For dangerous permissions, Android applications should check whether permission has already been granted.

Example:

```kotlin
if (ContextCompat.checkSelfPermission(
        this,
        Manifest.permission.CALL_PHONE
    ) != PackageManager.PERMISSION_GRANTED
) {
    ActivityCompat.requestPermissions(
        this,
        arrayOf(Manifest.permission.CALL_PHONE),
        100
    )
}
```

The permission result should then be handled appropriately.

For modern applications, Android recommends using the **Activity Result API** for permission requests.

---

# ActivityResultContracts

The Activity Result API provides a modern way to receive results from Activities and request permissions.

Examples include:

```kotlin
ActivityResultContracts.RequestPermission()
```

```kotlin
ActivityResultContracts.GetContent()
```

```kotlin
ActivityResultContracts.TakePicture()
```

Example:

```kotlin
private val requestCameraPermission =
    registerForActivityResult(
        ActivityResultContracts.RequestPermission()
    ) { granted ->
        if (granted) {
            // Permission granted
        }
    }
```

---

# Important Android Classes

This practical demonstrates the following Android classes and methods:

* `Intent`
* `Uri`
* `Intent.setData()`
* `Intent.setType()`
* `startActivity()`
* `ContextCompat.checkSelfPermission()`
* `ActivityCompat.requestPermissions()`
* `ActivityResultContracts`
* `ContactsContract`
* `CallLog`
* `AlarmClock`
* `MediaStore`

---

# Important Constants

## Telephone URI

```text
tel:
```

## Image MIME Type

```text
image/*
```

## Call Log Content Type

```kotlin
CallLog.Calls.CONTENT_TYPE
```

## Contacts Content Type

```kotlin
ContactsContract.Contacts.CONTENT_TYPE
```

---

# Difference Between Implicit and Explicit Intent

| Feature     | Implicit Intent               | Explicit Intent               |
| ----------- | ----------------------------- | ----------------------------- |
| Destination | Not directly specified        | Explicitly specified          |
| Component   | Android determines component  | Developer specifies component |
| Example     | Open browser                  | Open LoginActivity            |
| Common Use  | Communication with other apps | Navigation within application |

### Implicit Intent Example

```kotlin
val intent = Intent(
    Intent.ACTION_VIEW,
    Uri.parse("https://www.google.com")
)
startActivity(intent)
```

### Explicit Intent Example

```kotlin
val intent = Intent(this, LoginActivity::class.java)
startActivity(intent)
```

---

# Suggested Project Structure

```text
app/
└── src/
    └── main/
        ├── java/
        │   └── .../
        │       ├── MainActivity.kt
        │       └── LoginActivity.kt
        │
        ├── res/
        │   ├── drawable/
        │   ├── layout/
        │   │   ├── activity_main.xml
        │   │   └── activity_login.xml
        │   └── values/
        │
        └── AndroidManifest.xml
```

---

# Testing

After running the application, test each button individually.

### Test 1 — Make Call

Tap **Make Call** and verify that the phone/call interface opens for the specified number.

### Test 2 — Open URL

Tap **Open URL** and verify that the specified website opens in a browser.

### Test 3 — Open Call Log

Tap **Open Call Log** and verify that the Call Log application/interface opens.

### Test 4 — Open Gallery

Tap **Open Gallery** and verify that an image/gallery application opens.

### Test 5 — Set Alarm

Tap **Set Alarm** and verify that the alarm application opens with the specified alarm details.

### Test 6 — Open Camera

Tap **Open Camera** and verify that the camera interface opens.

### Test 7 — Open Login Activity

Tap **Open Login Activity** and verify that `LoginActivity` opens inside the application.

---

# Study Topics

The following topics should be studied as part of this practical:

* Intent
* Implicit Intent
* Explicit Intent
* Intent Actions
* `Intent.setData()`
* `Intent.setType()`
* `Uri.parse()`
* `startActivity()`
* Button
* ConstraintLayout
* CoordinatorLayout
* Activity Result API
* `ActivityResultContracts`
* Android permissions
* Manifest permissions
* Runtime permissions
* `ContextCompat.checkSelfPermission()`
* `ActivityCompat.requestPermissions()`
* `ContactsContract.Contacts.CONTENT_TYPE`
* `CallLog.Calls.CONTENT_TYPE`
* `image/*`
* `tel:`

---

# Learning Outcomes

After completing this practical, the student should be able to:

1. Understand the purpose of Android Intents.
2. Differentiate between implicit and explicit Intents.
3. Launch external applications using implicit Intents.
4. Navigate between Activities using explicit Intents.
5. Use `Uri.parse()` for URI-based Intent data.
6. Use `setData()` and `setType()`.
7. Work with telephone, web, gallery, camera, call-log, and alarm Intents.
8. Understand Android permissions.
9. Request runtime permissions safely.
10. Use the Activity Result API.
11. Design the application interface using `ConstraintLayout` or `CoordinatorLayout`.

---

# Conclusion

This practical demonstrates how Android applications communicate with other applications and Activities using **Implicit and Explicit Intents**. It also introduces Intent actions, URI and MIME-type handling, runtime permissions, and the modern Activity Result API.

The completed application provides practical examples of calling a number, opening a website, viewing the Call Log, opening the Gallery, setting an alarm, launching the Camera, and navigating to a Login Activity.
