```swift

//
//  ContentView.swift
//  FontList
//
//  Created by paige on 2022/01/30.
//

import SwiftUI

struct ContentView: View {
    
    @State var searchText: String = ""
    let alphabet: String = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    
    /*
     [
     "A": ["HELLO", "WORLD"],
     "B": ["HELLO", "WORLD"]
     ]
     */
    var fonts: [String: [String]] = [:]
    
    init() {
        UIFont.familyNames.forEach { fontName in
            for character in alphabet {
                if fontName == "Bodoni Ornaments" {
                    continue
                }
                if fontName.first == character {
                    if fonts.keys.contains(String(character)) {
                        fonts[String(character)]?.append(fontName)
                    } else {
                        fonts[String(character)] = [fontName]
                    }
                }
            }
        }
    }
    
    
    var body: some View {
        ScrollViewReader { proxy in
            
            ZStack {
                List {
                    SearchBar(text: $searchText)
                        .padding(EdgeInsets(top: 0, leading: -20, bottom: 0, trailing: -20))
                    ForEach(fonts.map{ $0.key }.sorted(by: <), id: \.self) { title in
                        Section(header: Text(title)) {
                            ForEach(fonts[title].map { $0 }!, id: \.self) { fontName in
                                Text(fontName)
                                    .font(.custom(fontName, size: 16))
                            }
                        }
                        .id(title)
                    }
                }
                .listStyle(GroupedListStyle())
                 
                VStack {
                    ForEach(fonts.keys.map { String($0) }.sorted(by: <), id: \.self) { letter in
                          HStack {
                              Spacer()
                              Button(action: {
                                  print("letter = \(letter)")
                                  //need to figure out if there is a name in this section before I allow scrollto or it will crash
                                  if fonts.keys.first(where: { letter == $0 }) != nil {
                                      withAnimation {
                                          proxy.scrollTo(letter)
                                      }
                                  }
                              }, label: {
                                  Text(letter)
                                      .font(.system(size: 12))
                                      .padding(.trailing, 7)
                              })
                          }
                      }
                  }
            }

        }
        
        
    }
    
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}


```
