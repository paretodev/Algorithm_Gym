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

### 1.14 (목)
### #DP #swift
<br>

```swift

import Foundation

let skyScrapers = [ 2, 9, 2, 3, 5, 3, 2, 8, 10, 12, 1, 3, 5, 8, 1, 3, 13 ]

//MARK:- ~번째 건물에서 오른쪽으로 보이는 건물 수
var dp_right = Array.init(repeating: 0, count: skyScrapers.count)
print(dp_right)

//MARK:- ~번째 건물에서 왼쪽으로 보이는 건물 수
var dp_left = Array.init(repeating: 0, count: skyScrapers.count)
print(dp_left)

//MARK:- 다이나믹 방식으로, 맨 오른쪽부터 왼쪽으로 이터레이션 하면서, 해당 인덱스 빌딩 우측으로 최초로 자기보다 더 높은 건물이 나오는 즉시
    // 그 건물의 디피값( 그 건물로 부터 오른쪽으로 보이는 건물의 수 )에 그 최초로 등장한 건물이 현재 인덱스 건물보다 높으니 +1 하여 dp_right에 기록
let rightIterOrder = Array( 0..<skyScrapers.count ).reversed()
outerLoop : for idx in rightIterOrder {
    if idx != skyScrapers.count - 1 {
        let tempIterOrder = Array( idx..<skyScrapers.count )
        for k in tempIterOrder {
            if skyScrapers[idx] < skyScrapers[k] {
                dp_right[idx] = dp_right[k] + 1
                continue outerLoop
            }
        }
    }
}
print(dp_right)

//MARK:- 다이나믹 방식으로, 맨 왼쪽부터 오른쪽으로 이터레이션 하면서, 해당 인덱스 빌딩 좌측으로 최초로 자기보다 더 높은 건물이 나오는 즉시
    // 그 건물의 디피값( 그 건물로 부터 오른쪽으로 보이는 건물의 수 )에다가, 그 최초의 건물이 현 인덱스 빌딩보다 높으니 +1 하여 dp_left에 기록하여 연산 횟수 줄이기
let leftIterOrder = Array( 0..<skyScrapers.count )
outerLoop : for idx in leftIterOrder {
    if idx != 0 {
        let tempIterOrder = Array( 0..<idx ).reversed()
        for k in tempIterOrder {
            if skyScrapers[idx] < skyScrapers[k] {
                dp_left[idx] = dp_left[k] + 1
                continue outerLoop
            }
        }
    }
}
print(dp_left)

var result = Array.init(repeating: 0, count: skyScrapers.count)
for i in 0..<result.count {
    result[i] = dp_right[i] + dp_left[i]
}

print(result)

var total = 0
for i in result {
    total += 1
}
print(total)

```
<br>

![](./images/2021-01-15-23-44-23.png)
<br>


