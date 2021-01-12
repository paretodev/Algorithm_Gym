# Algorithm_Problem_Solving

### Started Migration to github
[algorithms problem solving](https://www.notion.so/fundamentaldeveloper/8a085ab32c2c4e3bb15825916d3abd1a)

### 1.12 (화)
### 랭킹 산정하기

```swift
import Foundation

func array2GSB( _ array : [Int] ) -> [Int]{
    var tempArr = array.sorted()
    tempArr = tempArr.filter{ $0 != 0 }
    let totalNum = tempArr.count
    //
    let numberOf15 = ceil( ( Double(totalNum) * 0.15 ) )
    let numberOf30 = ceil( ( Double(totalNum) * 0.30 ) )
    let numberOf45 = ceil( ( Double(totalNum) * 0.45 ) )
    //
    let score15 = tempArr[Int(numberOf15) - 1]
    let score30 = tempArr[Int(numberOf30) - 1]
    let score45 = tempArr[Int(numberOf45) - 1]
    //
    return [ score15, score30, score45 ]
    //
}

let records = [
"jay:40,30,0,20,50,10",
"liaa:15,0,20,60,70,40",
"han:20,60,70,80,90,100",
"kim:0,15,0,10,5,0",
"json:0,70,60,0,15,2",
"niam:3,33,13,35,62,79",
"kia:34,81,19,31,6,7",
"tsla:91,9,55,3,62,71",
"tyta:1,92,5,32,61,0"
]

//
var name2trackRecords : [ String:[Int] ] = [:]
var track2allRecords: [Int : [Int]] = [:]

//
for string in records {
    //
    let results = string.split(separator: ":")
    //
    let list = results[1].split(separator: ",")
    let newList = list.map{
        Int( String($0) )!
    }
    //
    name2trackRecords[ String(results[0]) ] = newList
    //
    newList.enumerated().forEach{ idx, item in
        
        if track2allRecords.keys.contains(idx) {
            track2allRecords[idx]!.append(item)
        }else{
            track2allRecords[idx] = [item]
        }
    }
    //
    print(name2trackRecords)
    print(track2allRecords)
    // track number to [gold, silver, bronze]
}

//MARK:- Track# to gsb
var track2gsb : [Int : [Int]] = [:]
track2allRecords.forEach { key, arr in
    track2gsb[key] = array2GSB( arr )
}

// MARK: -Iterate people2recs and make ( done, g, s, b, name )
var resultTuples : [ (Int, Int, Int, Int, String) ] = []
name2trackRecords.forEach{ key, arr in
    
    var done = 0
    var gold = 0
    var sil = 0
    var bro = 0
    
    arr.enumerated().forEach{ idx, rec in
        
        let gsbLines = track2gsb[idx]!
        
        if rec == 0 {}
        else{
            done += 1
            if gsbLines[0] >= rec {
                gold += 1
            }
            else if gsbLines[1] >= rec{
                sil += 1
            }
            else if gsbLines[2] >= rec {
                bro += 1
            }
        }
        
        //
    }
    
    resultTuples.append( (done, gold, sil, bro, key)  )
    
}

resultTuples.sort{

    if $0.0 !=  $1.0 {
        return $0.0 > $1.0
    }
    if $0.1 !=  $1.1 {
        return $0.1 > $1.1
    }
    if $0.2 !=  $1.2 {
        return $0.2 > $1.2
    }
    if $0.3 !=  $1.3 {
        return $0.3 > $1.3
    }
    if $0.4 !=  $1.4 {
        return $0.0 < $1.0
    }
    
    return true
}

for tuple in resultTuples {
    print(tuple.4)
}
```
