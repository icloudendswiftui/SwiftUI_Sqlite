# Swift read data from sqlite data and display

## Introduction
***
Sqlite code is from [FahimF's SQLiteDB](https://github.com/FahimF/SQLiteDB). I modified it and added the custom table name function.

SwiftUI_Sqlite is a complete project that implements the functions of reading sqlite and displaying data in swiftui

## Features
- Features
- A pure-Swift interface
- Custom table name
- SwiftUI show data

## Usage
***
1. Adding to Your Project
(1) copy file to your project

```
SQLiteBbase.swift
SQLiteDB.swift
SQLTable

```
2. Direct
add init code to AppDelegate.swfit

```
    let db = SQLiteDB.shared
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        db.DB_NAME="landmarkdemo.db"
        _ = db.open(copyFile:true)
               
        return true
    }
```
3. create class
if you want to custom table name,you can set custom name with the following code 
```
 static func customTables() ->String {
        return "landmark"
    }
```
if you want to  use class name as table name ,you need use the following code
```
 static func customTables() ->String {
        return ""
    }
```
class code
```
import Foundation
import SwiftUI

class SQLandmark: SQLTable {
    var id = -1
    var name = ""
    var imageName = ""
    
    override var description:String {
        return "id: \(id), name: \(name)"
    }
   
    
    static func customTables() ->String {
        return "landmark"
    }
    
    
 
}

extension SQLandmark {
    var image: Image {
        ImageStore.shared.image(name: imageName)
    }
}

```

4、 create data.swift to init data
```
import Foundation
import SwiftUI

let sqLandmarkData: [SQLandmark] = SQLandmark.rows(order:"id ASC")
```

5、 create UI by SwiftUI
```
import SwiftUI

struct ListSqliteView: View {
    var body: some View {
        NavigationView {
            List{
                ForEach(sqLandmarkData,id:\.self){ item in
                    //Text(item.name)
                     SQLMRow(landmark: item)
                    
                }
                .navigationBarTitle(Text("Landmarks"))
            }
        }
    }
}

```

```
import SwiftUI

struct SQLMRow: View {
    var landmark: SQLandmark
    
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
```