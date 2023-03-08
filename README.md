# [go back to Overview](https://github.com/c4arl0s)

# [BuildingListAndNavigation - Content](https://github.com/c4arl0s/buildinglistandnavigation#go-back-to-overview)

1. [x] [1. Create a Landmark Model](https://github.com/c4arl0s/buildinglistandnavigation#1-Create-a-Landmark-Model)
2. [x] [2. Create the Row View](https://github.com/c4arl0s/buildinglistandnavigation#2-Create-the-Row-View)
3. [x] [3. Customize the Row Preview](https://github.com/c4arl0s/buildinglistandnavigation#3-Customize-the-Row-Preview)
4. [x] [4. Create the List of Landmarks](https://github.com/c4arl0s/buildinglistandnavigation#4-Create-the-List-of-Landmarks)
5. [x] [5. Make the List Dynamic](https://github.com/c4arl0s/buildinglistandnavigation#5-Make-the-List-Dynamic)
6. [x] [6. Set Up Navigation Between List and Detail](https://github.com/c4arl0s/buildinglistandnavigation#6-set-up-navigation-between-list-and-detail)
7. [x] [7. Pass Data into Child Views](https://github.com/c4arl0s/buildinglistandnavigation#7-pass-data-into-child-views)
8. [ ] [8. Generate Previews Dynamically](https://github.com/c4arl0s/buildinglistandnavigation#8-generate-previews-dynamically)

# [BuildingListAndNavigation](https://github.com/c4arl0s/buildinglistandnavigation#buildinglistandnavigation---content)

# 1. [Create a Landmark Model](https://github.com/c4arl0s/buildinglistandnavigation#buildinglistandnavigation---content)

In the first tutorial, you hard-coded information into all of your custom views. Here, you’ll create a model to store data that you can pass into your views.

Use the completed project from the previous tutorial and the resources available from this tutorial’s project files to get started.

# Step 1

Drag `landmarkData.json` in the downloaded files Resources folder into your project’s navigation pane; in the dialog that appears, select **“Copy items if needed”** and the Landmarks target, and then click Finish.

You’ll use this sample data throughout the remainder of this tutorial, and for all that follow.

# Step 2

Choose `File > New > File` to create a new Swift file in your project, and name it `Landmark.swift`.

# Step 3

Define a Landmark structure with a few properties matching names of some of the keys in the `landmarkData.json` data file.

Adding Codable conformance makes it easier to move data between the structure and a data file. You’ll rely on the `Decodable` component of the `Codable` protocol later in this section to read data from file.

```swift
import Foundation

struct Landmark: Hashable, Codable {
    var id: Int
    var name: String
    var park: String
    var state: String
    var description: String
}
```

In the next few steps you’ll model the image associated with each landmark.

# Step 4

Drag the JPG files from the project files’ Resources folder into your project’s asset catalog. Xcode creates a new image set for each image.

The new images join the one for Turtle Rock that you added in the previous tutorial.

# Step 5

Add an `imageName` property to read the name of the image from the data, and a computed image property that loads an image from the asset catalog.

```swift
import Foundation
import SwiftUI

struct Landmark: Hashable, Codable {
    var id: Int
    var name: String
    var park: String
    var state: String
    var description: String
    
    private var imageName: String
    var image: Image {
        Image(imageName)
    }
}
```

# Step 6

Add a coordinates property to the structure using a nested Coordinates type that reflects the storage in the JSON data structure.

```swift
import Foundation
import SwiftUI

struct Landmark: Hashable, Codable {
    var id: Int
    var name: String
    var park: String
    var state: String
    var description: String
    
    private var imageName: String
    var image: Image {
        Image(imageName)
    }
    
    private var coordinates: Coordinates
    
    struct Coordinates: Hashable, Codable {
        var latitude: Double
        var longitude: Double
    }
}
```

You mark this property as private because you’ll use it only to create a public computed property in the next step.

# Step 7

Compute a `locationCoordinate` property that’s useful for interacting with the `MapKit` framework.

```swift
import Foundation
import SwiftUI
import CoreLocation

struct Landmark: Hashable, Codable {
    var id: Int
    var name: String
    var park: String
    var state: String
    var description: String
    
    private var imageName: String
    var image: Image {
        Image(imageName)
    }
    
    private var coordinates: Coordinates
    
    struct Coordinates: Hashable, Codable {
        var latitude: Double
        var longitude: Double
    }
    
    var locationCoordinate: CLLocationCoordinate2D {
        CLLocationCoordinate2D(latitude: coordinates.latitude, longitude: coordinates.longitude)
    }
}
```

# Step 8

Create a new Swift file in your project and name it `ModelData.swift`.

# Step 9
Create a `load(_:)` method that fetches JSON data with a given name from the app’s main bundle.
The load method relies on the return type’s conformance to the `Decodable` protocol, which is one component of the `Codable` protocol.

```swift
import Foundation

func load<T: Decodable>(_ filename: String) -> T {
    let data: Data

    guard let file = Bundle.main.url(forResource: filename, withExtension: nil)
    else {
        fatalError("Couldn't find \(filename) in main bundle.")
    }

    do {
        data = try Data(contentsOf: file)
    } catch {
        fatalError("Couldn't load \(filename) from main bundle:\n\(error)")
    }

    do {
        let decoder = JSONDecoder()
        return try decoder.decode(T.self, from: data)
    } catch {
        fatalError("Couldn't parse \(filename) as \(T.self):\n\(error)")
    }
}
```

The load method relies on the return type’s conformance to the `Decodable` protocol, which is one component of the `Codable` protocol.

# Step 10

Create an array of landmarks that you initialize from `landmarkData.json`.

```swift
import Foundation

var landmarks: [Landmark] = load("landmarkData.json")

func load<T: Decodable>(_ filename: String) -> T {
    let data: Data

    guard let file = Bundle.main.url(forResource: filename, withExtension: nil)
    else {
        fatalError("Couldn't find \(filename) in main bundle.")
    }

    do {
        data = try Data(contentsOf: file)
    } catch {
        fatalError("Couldn't load \(filename) from main bundle:\n\(error)")
    }

    do {
        let decoder = JSONDecoder()
        return try decoder.decode(T.self, from: data)
    } catch {
        fatalError("Couldn't parse \(filename) as \(T.self):\n\(error)")
    }
}
```

Before moving on, group related files together to make it easier to manage your growing project.

# Step 11

Put `ContentView.swift`, `CircleImage.swift`, and `MapView.swift` into a Views group, `landmarkData.json` in a Resources group, and `Landmark.swift` and `ModelData.swift` into a Model group.

> Tip: You can create groups of existing items by selecting the items to add to the group, and then choosing `File > New > Group` from Selection in the Xcode menu.

# 2. [Create the Row View](https://github.com/c4arl0s/buildinglistandnavigation#buildinglistandnavigation---content)

The first view you’ll build in this tutorial is a row for displaying details about each landmark. This row view stores information in a property for the landmark it displays, so that one view can display any landmark. Later, you’ll combine multiple rows into a list of landmarks.

<img width="435" alt="Screenshot 2023-02-21 at 9 22 46 p m" src="https://user-images.githubusercontent.com/24994818/220513289-673a781c-0a7c-4f5a-a43f-8871313bc7d0.png">

# Step 1

Create a new SwiftUI view in the Views group named `LandmarkRow.swift`.

# Step 2

If the preview isn’t visible already, show the canvas by choosing `Editor > Canvas`, and then click `Resume`.

# Step 3

Add `landmark` as a stored property of `LandmarkRow`.

> When you add the `landmark` property, the preview stops working, because the `LandmarkRow` type needs a `landmark` instance during initialization.

```swift
struct LandmarkRow: View {
    var landmark: Landmark
    var body: some View {
        Text(/*@START_MENU_TOKEN@*/"Hello, World!"/*@END_MENU_TOKEN@*/)
    }
}

struct LandmarkRow_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkRow()
    }
}
```

<img width="518" alt="Screenshot 2023-02-21 at 9 33 03 p m" src="https://user-images.githubusercontent.com/24994818/220514659-0a895365-c214-4fcd-88c7-9ce4c1d235ba.png">

> To fix the preview, you’ll need to modify the preview provider.

# Step 4

In the previews `static` property of `LandmarkRow_Previews`, add the `landmark` parameter to the `LandmarkRow` initializer, specifying the first element of the landmarks array.

```swift
import SwiftUI

struct LandmarkRow: View {
    var landmark: Landmark
    var body: some View {
        Text(/*@START_MENU_TOKEN@*/"Hello, World!"/*@END_MENU_TOKEN@*/)
    }
}

struct LandmarkRow_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkRow(landmark: landmarks[0])
    }
}
```

With that fixed, you can build the layout for the row.

<img width="1086" alt="Screenshot 2023-02-21 at 9 51 17 p m" src="https://user-images.githubusercontent.com/24994818/220517278-9817251d-b3d8-478d-8ed8-dec75e34e86f.png">

# Step 5

Embed the existing text view in an `HStack`.

```swift
import SwiftUI

struct LandmarkRow: View {
    var landmark: Landmark
    var body: some View {
        HStack {
            Text("Hello, World!")
        }
    }
}

struct LandmarkRow_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkRow(landmark: landmarks[0])
    }
}
```

# Step 6

Modify the text view to use the landmark property’s name.

```swift
import SwiftUI

struct LandmarkRow: View {
    var landmark: Landmark
    var body: some View {
        HStack {
            Text(landmark.name)
        }
    }
}

struct LandmarkRow_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkRow(landmark: landmarks[0])
    }
}
```

<img width="935" alt="Screenshot 2023-02-21 at 9 56 54 p m" src="https://user-images.githubusercontent.com/24994818/220518044-f05ce3a0-4d24-4759-9939-82567c2e6573.png">

# Step 7

Complete the row by adding an image before the text view, and a spacer after it.

```swift
import SwiftUI

struct LandmarkRow: View {
    var landmark: Landmark
    var body: some View {
        HStack {
            landmark.image
                .resizable()
                .frame(width: 50, height: 50)
            Text(landmark.name)
            Spacer()
        }
    }
}

struct LandmarkRow_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkRow(landmark: landmarks[0])
    }
}
```

<img width="983" alt="Screenshot 2023-02-21 at 9 59 52 p m" src="https://user-images.githubusercontent.com/24994818/220518445-23f1bc00-63c8-4048-ad4c-9c5b950a0e08.png">

# 3. [Customize the Row Preview](https://github.com/c4arl0s/buildinglistandnavigation#buildinglistandnavigation---content)

Xcode’s canvas automatically recognizes and displays any type in the current editor that conforms to the `PreviewProvider` protocol. A preview provider returns one or more views, with options to configure the size and device.

You can customize the returned content from a preview provider to render exactly the previews that are most helpful to you.

<img width="432" alt="Screenshot 2023-02-22 at 9 48 06 p m" src="https://user-images.githubusercontent.com/24994818/220817314-ed7af686-0235-4061-a035-ce307edccb18.png">

# Step 1

In `LandmarkRow_Previews`, update the landmark parameter to be the second element in the `landmarks` array.

```swift
import SwiftUI

struct LandmarkRow: View {
    var landmark: Landmark
    var body: some View {
        HStack {
            landmark.image
                .resizable()
                .frame(width: 50, height: 50)
            Text(landmark.name)
            Spacer()
        }
    }
}

struct LandmarkRow_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkRow(landmark: landmarks[1])
    }
}
```

The preview immediately changes to show the second sample landmark instead of the first.

<img width="851" alt="Screenshot 2023-02-22 at 9 50 43 p m" src="https://user-images.githubusercontent.com/24994818/220817550-f07933ee-7718-4910-a32d-329fa2450062.png">

# Step 2

Use the `previewLayout(_:)` modifier to set a `size` that approximates a row in a list.

```swift
import SwiftUI

struct LandmarkRow: View {
    var landmark: Landmark
    var body: some View {
        HStack {
            landmark.image
                .resizable()
                .frame(width: 50, height: 50)
            Text(landmark.name)
            Spacer()
        }
    }
}

struct LandmarkRow_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkRow(landmark: landmarks[1])
            .previewLayout(.fixed(width: 300, height: 70))
    }
}
```

You can use a `Group` to return multiple previews from a preview provider.

<img width="844" alt="Screenshot 2023-02-22 at 9 54 00 p m" src="https://user-images.githubusercontent.com/24994818/220817925-beb967cf-bb84-4dd5-b27e-910e9453d6b9.png">

# Step 3

Wrap the returned row in a `Group`, and add the first row back again.

```swift
import SwiftUI

struct LandmarkRow: View {
    var landmark: Landmark
    var body: some View {
        HStack {
            landmark.image
                .resizable()
                .frame(width: 50, height: 50)
            Text(landmark.name)
            Spacer()
        }
    }
}

struct LandmarkRow_Previews: PreviewProvider {
    static var previews: some View {
        Group {
            LandmarkRow(landmark: landmarks[0])
                .previewLayout(.fixed(width: 300, height: 70))
            LandmarkRow(landmark: landmarks[1])
                .previewLayout(.fixed(width: 300, height: 70))
        }
    }
}
```

`Group` is a container for grouping view content. Xcode renders the group’s child views as separate previews in the canvas.

<img width="883" alt="Screenshot 2023-02-22 at 9 57 27 p m" src="https://user-images.githubusercontent.com/24994818/220818283-6d871563-e7be-4e04-8283-aca9eebfa0e8.png">

# Step 4

To simplify the code, move the `previewLayout(_:)` call to the outside of the group’s child declarations.

```swift
import SwiftUI

struct LandmarkRow: View {
    var landmark: Landmark
    var body: some View {
        HStack {
            landmark.image
                .resizable()
                .frame(width: 50, height: 50)
            Text(landmark.name)
            Spacer()
        }
    }
}

struct LandmarkRow_Previews: PreviewProvider {
    static var previews: some View {
        Group {
            LandmarkRow(landmark: landmarks[0])
            LandmarkRow(landmark: landmarks[1])
        }
        .previewLayout(.fixed(width: 300, height: 70))
    }
}
```

A view’s children inherit the view’s contextual settings, such as preview configurations.

The code you write in a preview provider only changes what Xcode displays in the canvas.

<img width="877" alt="Screenshot 2023-02-22 at 10 01 50 p m" src="https://user-images.githubusercontent.com/24994818/220818710-60e86583-4636-4c6c-a17f-6754872aad1a.png">

# 4. [Create the List of Landmarks](https://github.com/c4arl0s/buildinglistandnavigation#buildinglistandnavigation---content)

When you use SwiftUI’s List type, you can display a platform-specific list of views. The elements of the list can be static, like the child views of the stacks you’ve created so far, or dynamically generated. You can even mix static and dynamically generated views.

<img width="418" alt="Screenshot 2023-02-22 at 10 09 10 p m" src="https://user-images.githubusercontent.com/24994818/220819473-99022536-8427-4e19-a993-9e205665200a.png">

# Step 1

Create a new SwiftUI view in the Views group named `LandmarkList.swift`.

<img width="231" alt="Screenshot 2023-02-22 at 10 12 03 p m" src="https://user-images.githubusercontent.com/24994818/220819757-fb650451-94c5-49ed-9030-049613e8a7fe.png">

# Step 2

Replace the default `Text` view with a `List`, and provide `LandmarkRow` instances with the first two landmarks as the list’s children.

```swift
import SwiftUI

struct LandmarkList: View {
    var body: some View {
        List {
            LandmarkRow(landmark: landmarks[0])
            LandmarkRow(landmark: landmarks[1])
        }
    }
}

struct LandmarkList_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkList()
    }
}
```

The preview shows the two landmarks rendered in a list style that’s appropriate for iOS.

<img width="1045" alt="Screenshot 2023-02-22 at 10 26 04 p m" src="https://user-images.githubusercontent.com/24994818/220821234-06fa5020-0190-4004-8884-3c7a810442d0.png">

# 5. [Make the List Dynamic](https://github.com/c4arl0s/buildinglistandnavigation#buildinglistandnavigation---content)

Instead of specifying a list’s elements individually, you can generate rows directly from a collection.

You can create a list that displays the elements of a collection by passing your collection of data and a closure that provides a view for each element in the collection. The list transforms each element in the collection into a child view by using the supplied closure.

<img width="365" alt="Screenshot 2023-02-22 at 10 33 58 p m" src="https://user-images.githubusercontent.com/24994818/220822064-255b711c-3720-40f4-ba01-aeb4eaa13a55.png">

# Step 1

Remove the two static landmark rows, and instead pass the model data’s landmarks array to the `List` initializer.

```swift
import SwiftUI

struct LandmarkList: View {
    var body: some View {
        List(landmarks, id: \.id) { landmark in
            
        }
    }
}

struct LandmarkList_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkList()
    }
}
```

Lists work with identifiable data. You can make your data identifiable in one of two ways: 

1. by passing along with your data a key path to a property that uniquely identifies each element, 
2. or by making your data type conform to the `Identifiable` protocol.

<img width="1044" alt="Screenshot 2023-02-22 at 10 39 21 p m" src="https://user-images.githubusercontent.com/24994818/220822595-fc189677-9f7c-4a5c-a00f-b312aa018481.png">

# Step 2

Complete the dynamically-generated list by returning a LandmarkRow from the closure.

```swift
import SwiftUI

struct LandmarkList: View {
    var body: some View {
        List(landmarks, id: \.id) { landmark in
            LandmarkRow(landmark: landmark)
        }
    }
}

struct LandmarkList_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkList()
    }
}
```

This creates one `LandmarkRow` for each element in the landmarks array.

<img width="1032" alt="Screenshot 2023-02-22 at 10 42 34 p m" src="https://user-images.githubusercontent.com/24994818/220822913-c92fc1f8-880f-4712-8a9c-910385678092.png">

Next, you’ll simplify the List code by adding `Identifiable` conformance to the `Landmark` type.

# Step 3

Switch to `Landmark.swift` and declare conformance to the `Identifiable` protocol.

The `Landmark` data already has the `id` property required by `Identifiable` protocol; you only need to add a property to decode it when reading the data.

```swift
import Foundation
import SwiftUI
import CoreLocation

struct Landmark: Hashable, Codable, Identifiable {
    var id: Int
    var name: String
    var park: String
    var state: String
    var description: String

    private var imageName: String
    var image: Image {
        Image(imageName)
    }

    private var coordinates: Coordinates
    var locationCoordinate: CLLocationCoordinate2D {
        CLLocationCoordinate2D(
            latitude: coordinates.latitude,
            longitude: coordinates.longitude)
    }

    struct Coordinates: Hashable, Codable {
        var latitude: Double
        var longitude: Double
    }
}
```

# Step 4

Switch back to `LandmarkList.swift` and remove the `id` parameter.

```swift
import SwiftUI

struct LandmarkList: View {
    var body: some View {
        List(landmarks) { landmark in
            LandmarkRow(landmark: landmark)
        }
    }
}

struct LandmarkList_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkList()
    }
}
```

<img width="1046" alt="Screenshot 2023-02-22 at 11 34 32 p m" src="https://user-images.githubusercontent.com/24994818/220828449-2fd60237-d9ff-4622-8376-fd586e47fe70.png">

From now on, you’ll be able to use collections of `Landmark` elements directly.

# 6. [Set Up Navigation Between List and Detail]()

The list renders properly, but you can’t tap an individual landmark to see that landmark’s detail page yet.

You add navigation capabilities to a list by embedding it in a `NavigationView`, and then nesting each row in a `NavigationLink` to set up a transtition to a destination view.

<img width="462" alt="Screenshot 2023-02-23 at 10 15 24 p m" src="https://user-images.githubusercontent.com/24994818/221090378-9c367f86-c552-4fce-a73d-992867d9366f.png">

Prepare a detail view using the content you created in the previous tutorial and update the main content view to display the list view instead.

# Step 1

Create a new SwiftUI view named `LandmarkDetailView.swift`.

# Step 2

Copy the contents of the body property from `ContentView` into `LandmarkDetail`.

```swift
import SwiftUI

struct LandmarkDetailView: View {
    var body: some View {
        VStack {
            MapView()
                .ignoresSafeArea(edges: .top)
                .frame(height: 300)
            
            CircleImage()
                .offset(y: -130)
                .padding(.bottom, -130)
            
            VStack(alignment: .leading) {
                Text("Turtle, Rock!")
                    .font(.title)
                    .foregroundColor(.blue)
                HStack {
                    Text("Joshua Tree National Park")
                        .font(.subheadline)
                    Spacer()
                    Text("California")
                        .font(.subheadline)
                }
                .font(.subheadline)
                .foregroundColor(.secondary)
                
                Divider()
                
                Text("About Turtle Rock")
                    .font(.title2)
                
                Text("Description goes here")
            }
            .padding()
            
            Spacer()
        }
    }
}

struct LandmarkDetailView_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkDetailView()
    }
}
```

<img width="914" alt="Screenshot 2023-02-23 at 10 24 05 p m" src="https://user-images.githubusercontent.com/24994818/221091320-eb337b13-bfa1-455b-8558-8684bf50dc11.png">

# Step 3

Change `ContentView` to instead display `LandmarkList`

```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        LandmarkList()
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

In the next few steps, you’ll add navigation among your list and detail views.

# Step 4

Embed the dynamically generated list of landmarks in a `NavigationView`.

```swift
import SwiftUI

struct LandmarkList: View {
    var body: some View {
        NavigationView {
            List(landmarks) { landmark in
                LandmarkRow(landmark: landmark)
            }
        }
    }
}

struct LandmarkList_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkList()
    }
}
```

# Step 5

Call the `navigationTitle(_:)` modifier method to set the `title` of the navigation bar when displaying the list.

```swift
import SwiftUI

struct LandmarkList: View {
    var body: some View {
        NavigationView {
            List(landmarks) { landmark in
                LandmarkRow(landmark: landmark)
            }
            .navigationTitle("Landmarks")
        }
    }
}

struct LandmarkList_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkList()
    }
}
```

<img width="992" alt="Screenshot 2023-02-23 at 10 41 23 p m" src="https://user-images.githubusercontent.com/24994818/221093419-6089fa12-6259-49e4-b500-ad9b2f2a63c1.png">

# Step 6

Inside the list’s closure, wrap the returned row in a `NavigationLink`, specifying the `LandmarkDetailView` view as the destination.

```swift
import SwiftUI

struct LandmarkList: View {
    var body: some View {
        NavigationView {
            List(landmarks) { landmark in
                NavigationLink {
                    LandmarkDetailView()
                } label: {
                    LandmarkRow(landmark: landmark)
                }
            }
            .navigationTitle("Landmarks")
        }
    }
}

struct LandmarkList_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkList()
    }
}
```

# Step 7

You can try out the navigation directly in the preview by switching to live mode. Click the Live Preview button and tap a landmark to visit the detail page.

![Screen Recording 2023-02-23 at 11 04 15 p m 2023-02-23 11_07_43 p m](https://user-images.githubusercontent.com/24994818/221096940-b6a1b109-fa58-4cf3-9086-c684a19fd5a7.gif)

# 7. [Pass Data into Child Views](https://github.com/c4arl0s/BuildingListAndNavigation#buildinglistandnavigation---content)

The `LandmarkDetail` view still uses hard-coded details to show its landmark. Just like `LandmarkRow`, the `LandmarkDetail` type and the views it comprises need to use a landmark property as the source for their data.

<img width="244" alt="Screenshot 2023-03-07 at 10 36 59 p m" src="https://user-images.githubusercontent.com/24994818/223620843-3427a932-1f9f-4ebd-bfb5-37d4b762bccb.png">

Starting with the child views, you’ll convert `CircleImage`, `MapView`, and then `LandmarkDetail` to display data that’s passed in, rather than hard-coding each row.

# Step 1

In `CircleImage.swift`, add a stored image property to `CircleImage`.

<img width="557" alt="Screenshot 2023-03-07 at 10 45 20 p m" src="https://user-images.githubusercontent.com/24994818/223621896-ac237213-a832-4512-85c0-fd432e13842f.png">

```swift
import SwiftUI

struct CircleImage: View {
    var image: Image
    
    var body: some View {
        image
            .clipShape(Circle())
            .overlay {
                Circle().stroke(.white, lineWidth: 4)
            }
            .shadow(radius: 7)
    }
}

struct CircleImage_Previews: PreviewProvider {
    static var previews: some View {
        CircleImage()
    }
}
```

This is a common pattern when building views using SwiftUI. Your custom views will often wrap and encapsulate a series of modifiers for a particular view.

# Step 2

Update the preview provider to pass the image of Turtle Rock.

<img width="636" alt="Screenshot 2023-03-07 at 10 54 12 p m" src="https://user-images.githubusercontent.com/24994818/223623009-cdf66fa5-3c77-4256-b37f-d4a82104750a.png">

Even though you’ve fixed the preview logic, the preview fails to update because the build fails. The detail view, which instantiates a circle image, needs an input parameter as well.

```swift
import SwiftUI

struct CircleImage: View {
    var image: Image
    
    var body: some View {
        image
            .clipShape(Circle())
            .overlay {
                Circle().stroke(.white, lineWidth: 4)
            }
            .shadow(radius: 7)
    }
}

struct CircleImage_Previews: PreviewProvider {
    static var previews: some View {
        CircleImage(image: Image("turtlerock"))
    }
}
```

# Step

In `MapView.swift`, add a coordinate property to `MapView` and update the preview provider to pass a fixed coordinate.

<img width="1187" alt="Screenshot 2023-03-07 at 11 02 13 p m" src="https://user-images.githubusercontent.com/24994818/223623990-8458262b-d876-4472-bc33-22adaa7c22d4.png">

```swift
import SwiftUI
import MapKit

struct MapView: View {
    var coordinate: CLLocationCoordinate2D
    @State private var region = MKCoordinateRegion(
        center: CLLocationCoordinate2D(latitude: 34.011_286, longitude: -116.166_868), span: MKCoordinateSpan(latitudeDelta: 0.2, longitudeDelta: 0.2)
    )
    
    var body: some View {
        Map(coordinateRegion: $region)
    }
}

struct MapView_Previews: PreviewProvider {
    static var previews: some View {
        MapView(coordinate: CLLocationCoordinate2D(latitude: 34.011_286, longitude: -116.166_868))
    }
}
```

This change also affects the build because the detail view has a map view that needs the new parameter. You’ll fix the detail view soon.

# Step 4

Add a method that updates the region based on a coordinate value.

```swift
import SwiftUI
import MapKit

struct MapView: View {
    var coordinate: CLLocationCoordinate2D
    @State private var region = MKCoordinateRegion(
        center: CLLocationCoordinate2D(latitude: 34.011_286, longitude: -116.166_868), span: MKCoordinateSpan(latitudeDelta: 0.2, longitudeDelta: 0.2)
    )
    
    var body: some View {
        Map(coordinateRegion: $region)
    }
    
    private func setRegion(_ coordinate: CLLocationCoordinate2D) {
        region = MKCoordinateRegion(
            center: coordinate,
            span: MKCoordinateSpan(latitudeDelta: 0.2, longitudeDelta: 0.2)
        )
    }
}

struct MapView_Previews: PreviewProvider {
    static var previews: some View {
        MapView(coordinate: CLLocationCoordinate2D(latitude: 34.011_286, longitude: -116.166_868))
    }
}
```

# Step 5

Add an onAppear view modifier to the map that triggers a calculation of the region based on the current coordinate.

```swift
import SwiftUI
import MapKit

struct MapView: View {
    var coordinate: CLLocationCoordinate2D
    @State private var region = MKCoordinateRegion(
        center: CLLocationCoordinate2D(latitude: 34.011_286, longitude: -116.166_868), span: MKCoordinateSpan(latitudeDelta: 0.2, longitudeDelta: 0.2)
    )
    
    var body: some View {
        Map(coordinateRegion: $region)
            .onAppear {
                setRegion(coordinate)
            }
    }
    
    private func setRegion(_ coordinate: CLLocationCoordinate2D) {
        region = MKCoordinateRegion(
            center: coordinate,
            span: MKCoordinateSpan(latitudeDelta: 0.2, longitudeDelta: 0.2)
        )
    }
}

struct MapView_Previews: PreviewProvider {
    static var previews: some View {
        MapView(coordinate: CLLocationCoordinate2D(latitude: 34.011_286, longitude: -116.166_868))
    }
}
```

# Step 6

In `LandmarkDetail.swift`, add a Landmark property to the `LandmarkDetail` type.

<img width="762" alt="Screenshot 2023-03-07 at 11 22 56 p m" src="https://user-images.githubusercontent.com/24994818/223626563-8411a61d-ab28-490a-ad7f-c2175d0fdd32.png">

```swift
import SwiftUI

struct LandmarkDetailView: View {
    var landmark: Landmark
    
    var body: some View {
        VStack {
            MapView()
                .ignoresSafeArea(edges: .top)
                .frame(height: 300)
            
            CircleImage()
                .offset(y: -130)
                .padding(.bottom, -130)
            
            VStack(alignment: .leading) {
                Text("Turtle, Rock!")
                    .font(.title)
                    .foregroundColor(.blue)
                HStack {
                    Text("Joshua Tree National Park")
                        .font(.subheadline)
                    Spacer()
                    Text("California")
                        .font(.subheadline)
                }
                .font(.subheadline)
                .foregroundColor(.secondary)
                
                Divider()
                
                Text("About Turtle Rock")
                    .font(.title2)
                
                Text("Description goes here")
            }
            .padding()
            
            Spacer()
        }
    }
}

struct LandmarkDetailView_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkDetailView(landmark: landmarks[0])
    }
}
```

# Step 7

In `LandmarkList.swift`, pass the current landmark to the destination `LandmarkDetail`.

```swift
import SwiftUI

struct LandmarkList: View {
    var body: some View {
        NavigationView {
            List(landmarks) { landmark in
                NavigationLink {
                    LandmarkDetailView(landmark: landmark)
                } label: {
                    LandmarkRow(landmark: landmark)
                }
            }
            .navigationTitle("Landmarks")
        }
    }
}

struct LandmarkList_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkList()
    }
}
```

# Step 8

In the `LandmarkDetailView` file, pass the required data to your custom types.

```swift
import SwiftUI

struct LandmarkDetailView: View {
    var landmark: Landmark
    
    var body: some View {
        VStack {
            MapView(coordinate: landmark.locationCoordinate)
                .ignoresSafeArea(edges: .top)
                .frame(height: 300)
            
            CircleImage(image: landmark.image)
                .offset(y: -130)
                .padding(.bottom, -130)
            
            VStack(alignment: .leading) {
                Text(landmark.name)
                    .font(.title)
                    .foregroundColor(.blue)
                HStack {
                    Text(landmark.park)
                        .font(.subheadline)
                    Spacer()
                    Text(landmark.state)
                        .font(.subheadline)
                }
                .font(.subheadline)
                .foregroundColor(.secondary)
                
                Divider()
                
                Text("About \(landmark.name)")
                    .font(.title2)
                
                Text(landmark.description)
            }
            .padding()
            
            Spacer()
        }
    }
}

struct LandmarkDetailView_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkDetailView(landmark: landmarks[0])
    }
}
```

<img width="520" alt="Screenshot 2023-03-07 at 11 58 07 p m" src="https://user-images.githubusercontent.com/24994818/223631461-8688143c-469e-40a4-a7f2-98a2a515402c.png">

# Step 9

Change the container from a `VStack` to a `ScrollView` so the user can scroll through the descriptive content, and delete the `Spacer`, which you no longer need.

```swift
import SwiftUI

struct LandmarkDetailView: View {
    var landmark: Landmark
    
    var body: some View {
        ScrollView {
            MapView(coordinate: landmark.locationCoordinate)
                .ignoresSafeArea(edges: .top)
                .frame(height: 300)
            
            CircleImage(image: landmark.image)
                .offset(y: -130)
                .padding(.bottom, -130)
            
            VStack(alignment: .leading) {
                Text(landmark.name)
                    .font(.title)
                    .foregroundColor(.blue)
                HStack {
                    Text(landmark.park)
                        .font(.subheadline)
                    Spacer()
                    Text(landmark.state)
                        .font(.subheadline)
                }
                .font(.subheadline)
                .foregroundColor(.secondary)
                
                Divider()
                
                Text("About \(landmark.name)")
                    .font(.title2)
                
                Text(landmark.description)
            }
            .padding()
            
            Spacer()
        }
    }
}

struct LandmarkDetailView_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkDetailView(landmark: landmarks[0])
    }
}
```

![scrollview](https://user-images.githubusercontent.com/24994818/223633005-ae5e2a85-a87a-469e-bd8d-90dd1ba32eaa.gif)

# Step 10

Finally, call the `navigationTitle(_:)` modifier to give the navigation bar a title when showing the detail view, and the `navigationBarTitleDisplayMode(_:)` modifier to make the title appear inline.

```swift
import SwiftUI

struct LandmarkDetailView: View {
    var landmark: Landmark
    
    var body: some View {
        ScrollView {
            MapView(coordinate: landmark.locationCoordinate)
                .ignoresSafeArea(edges: .top)
                .frame(height: 300)
            
            CircleImage(image: landmark.image)
                .offset(y: -130)
                .padding(.bottom, -130)
            
            VStack(alignment: .leading) {
                Text(landmark.name)
                    .font(.title)
                    .foregroundColor(.blue)
                HStack {
                    Text(landmark.park)
                        .font(.subheadline)
                    Spacer()
                    Text(landmark.state)
                        .font(.subheadline)
                }
                .font(.subheadline)
                .foregroundColor(.secondary)
                
                Divider()
                
                Text("About \(landmark.name)")
                    .font(.title2)
                
                Text(landmark.description)
            }
            .padding()
        }
        .navigationTitle(landmark.name)
        .navigationBarTitleDisplayMode(.inline)
    }
        
}

struct LandmarkDetailView_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkDetailView(landmark: landmarks[0])
    }
}
```

# Step 11 

Switch to the live preview to see the detail view show the correct landmarks when you navigate from the list.

<img width="520" alt="Screenshot 2023-03-08 at 12 17 07 a m" src="https://user-images.githubusercontent.com/24994818/223634571-6b5aeb67-5d9d-4f2f-a84c-09b987c5ff1d.png">

# 8. [Generate Previews Dynamically](https://github.com/c4arl0s/BuildingListAndNavigation#buildinglistandnavigation---content)

