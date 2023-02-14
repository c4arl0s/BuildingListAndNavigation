# [go back to Overview](https://github.com/c4arl0s)

# [BuildingListAndNavigation - Content](https://github.com/c4arl0s/buildinglistandnavigation#go-back-to-overview)

1. [x] [1. Create a Landmark Model](https://github.com/c4arl0s/buildinglistandnavigation#1-Create-a-Landmark-Model)
2. [ ] [2. Create the Row View](https://github.com/c4arl0s/buildinglistandnavigation#2-Create-the-Row-View)
3. [ ] [3. Customize the Row Preview](https://github.com/c4arl0s/buildinglistandnavigation#3-Customize-the-Row-Preview)
4. [ ] [4. Create the List of Landmarks](https://github.com/c4arl0s/buildinglistandnavigation#4-Create-the-List-of-Landmarks)
5. [ ] [5. Make the List Dynamic](https://github.com/c4arl0s/buildinglistandnavigation#5-Make-the-List-Dynamic)

# [BuildingListAndNavigation](https://github.com/c4arl0s/buildinglistandnavigation#buildinglistandnavigation---content)

# 1. [Create a Landmark Model](https://github.com/c4arl0s/buildinglistandnavigation#buildinglistandnavigation---content)

In the first tutorial, you hard-coded information into all of your custom views. Here, you’ll create a model to store data that you can pass into your views.

Use the completed project from the previous tutorial and the resources available from this tutorial’s project files to get started.

# Step 1

Drag landmarkData.json in the downloaded files’ Resources folder into your project’s navigation pane; in the dialog that appears, select **“Copy items if needed”** and the Landmarks target, and then click Finish.

You’ll use this sample data throughout the remainder of this tutorial, and for all that follow.

# Step 2

Choose `File > New > File` to create a new Swift file in your project, and name it `Landmark.swift`.

# Step 3

Define a Landmark structure with a few properties matching names of some of the keys in the landmarkData.json data file.

Adding Codable conformance makes it easier to move data between the structure and a data file. You’ll rely on the Decodable component of the Codable protocol later in this section to read data from file.

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

Add an imageName property to read the name of the image from the data, and a computed image property that loads an image from the asset catalog.

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

Compute a locationCoordinate property that’s useful for interacting with the MapKit framework.

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
The load method relies on the return type’s conformance to the Decodable protocol, which is one component of the Codable protocol.

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

The load method relies on the return type’s conformance to the Decodable protocol, which is one component of the Codable protocol.

# Step 10

Create an array of landmarks that you initialize from landmarkData.json.

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
# 3. [Customize the Row Preview](https://github.com/c4arl0s/buildinglistandnavigation#buildinglistandnavigation---content)
# 4. [Create the List of Landmarks](https://github.com/c4arl0s/buildinglistandnavigation#buildinglistandnavigation---content)
# 5. [Make the List Dynamic](https://github.com/c4arl0s/buildinglistandnavigation#buildinglistandnavigation---content)
