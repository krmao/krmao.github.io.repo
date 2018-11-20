---
title: 移动端_IOS_UICOLLECTIONVIEW_CELL_自适应大小
date: 2018-11-19 18:29:49
categories: [技术]
tags: [移动端,IOS]
---

### 目的
自己研究是一个痛苦的过程,网上的完整一点的文章很少,而且总是遇到各种各样的问题,所以记录下来去帮助那些像我一样的人少走一些弯路
### 实现目标 可以横向滚动的菜单栏
点击任意一个CELL ,下方的数据发生改变,这里先用横向滚动的UITableView,不好做,然后用第三方的UIScrollView做的,也不好用

![image.png](http://upload-images.jianshu.io/upload_images/2062311-65e0a7b6423957be.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 过程 
```
一个小小的自适应代码,搞了两天两夜,IOS相比Android编码,简直太落后了,

1. 通过约束自适应大小会出现最后1-2个数据不显示的问题,count是正确的,此时发现设置sectionInset>0 以及 minimumLineSpacing=1 就显示完整了,但是这样代码就变成了魔术代码,后经过多方请教,发现决定性因素是 返回 CELL size的时候不能写 zero, 宽度自适应的时候,设置宽>0,高度自适应的时候要设置高>0,不然会出现末尾的几个cell丢失的不明不白的问题,如果约束写在自定义CELL 的 layoutSubviews 方法里,则需要在显示CELL的时候即cellForItemAt方法里调用 cell.layoutIfNeeded(),不然会出现CELL错乱的问题

2. 通过自己计算CELL的大小,最开始是稀里糊涂的,结果现在已经豁然开朗,在 sizeForItemAt 里面返回的是 CELL.frame 的size ,但是CELL frame 里面的子view 都还没有设置大小,所以会不显示,需要在 layoutSubviews 里对子view进行设置大小,所以我认为 layoutSubviews 方法是在 sizeForItemAt之后调用的,已经可以拿到 CELL frame size

3. 总结: 约束自适应优先使用,因为CELL越来越复杂的话自己计算就会十分麻烦,当然也可以自己计算大小然后通过约束来搞定subView 之间的位置关系混着用
```

### 代码
```
import SnapKit
import Foundation

class HorizontalCollectionView: UIView, UICollectionViewDelegate, UICollectionViewDataSource, UICollectionViewDelegateFlowLayout {
    var dataArray: [String] = [] {
        didSet {
            collectionView.reloadData()
            collectionView.collectionViewLayout.invalidateLayout()
            //collectionView.reloadSections(IndexSet(integersIn: 0...0))
        }
    }
    var collectionView: UICollectionView!

    override init(frame: CGRect) {
        super.init(frame: frame)
        self.setUp()
    }

    required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }

    func requestViewData() {
        dataArray = ["0", "1", "22", "333", "4444", "55555", "666666", "7777777", "88888888", "999999999", "10101010101010101010", "A", "1111111111111111111111", "12", "13", "14"]
        print("dataArray.count=", dataArray.count)
    }

    private func setUp() {
        let flowLayout = UICollectionViewFlowLayout()

        flowLayout.minimumInteritemSpacing = 10;
        flowLayout.sectionInset = UIEdgeInsetsMake(0, 0, 0, 0)
        flowLayout.scrollDirection = .horizontal

        //===== CELL 约束自适应 必备条件 1 =====
        if #available(iOS 10.0, *) {
            flowLayout.estimatedItemSize = UICollectionViewFlowLayoutAutomaticSize
        } else {
            flowLayout.estimatedItemSize = CGSize(width: 15, height: 48)
        }

        collectionView = UICollectionView.init(frame: CGRect(0, 0, SCREEN_WIDTH, 48), collectionViewLayout: flowLayout)
        collectionView.delegate = self
        collectionView.dataSource = self
        collectionView.backgroundColor = UIColor.yellow
        collectionView.showsHorizontalScrollIndicator = false
        self.addSubview(collectionView)

        collectionView.register(HorizontalLabelCell.classForCoder(), forCellWithReuseIdentifier: "_cell")
    }

    func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {
        print("点击了 cell")
    }

    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        print("dataArray.count=", dataArray.count)
        return dataArray.count
    }

    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "_cell", for: indexPath) as! HorizontalLabelCell
        cell.title = dataArray[indexPath.row]
        return cell
    }

    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {
        //===== CELL 计算自适应 必备条件 1 =====
        // return CGSize(dataArray[indexPath.row].widthWithFont(), 48)

        //===== CELL 约束自适应 必备条件 2 ===== 宽度或者高度一定要  大于 0 ,否则会出现丢失错误等不可预料问题
        return CGSize(0.01, 48)
    }
}

class HorizontalLabelCell: UICollectionViewCell {

    private lazy var titleLabel: UILabel = {
        let label = UILabel()
        label.numberOfLines = 1
        label.font = UIFont.systemFont(ofSize: 18)
        label.textAlignment = .center
        label.textColor = UIColor.red
        return label
    }()

    var title: String = "" {
        didSet {
            self.titleLabel.text = title
        }
    }

    required init?(coder aDecoder: NSCoder) {
        super.init(coder: aDecoder)
    }

    override init(frame: CGRect) {
        super.init(frame: frame)
        self.contentView.addSubview(self.titleLabel)

        //===== CELL 约束自适应 必备条件 3 =====
        self.titleLabel.snp.makeConstraints { maker in
            maker.left.top.equalTo(self.contentView)
            maker.width.greaterThanOrEqualTo(5)
            maker.height.greaterThanOrEqualTo(48)
            maker.height.equalTo(self.contentView).priorityLow()
            maker.right.equalTo(self.contentView).priorityLow()
        }
    }

    //===== CELL 计算自适应 必备条件 2 ===== 如果没有使用约束,则在这里赋值 subViews frame 大小
    /*override func layoutSubviews() {
        super.layoutSubviews()
        self.titleLabel.frame = self.contentView.frame
    }*/

}

```

###使用
```
    lazy var topMenuView: HorizontalCollectionView = {
        let horizontalView = HorizontalCollectionView.init(frame: CGRect(0, 0, SCREEN_WIDTH, 48))
        return horizontalView
    }()

    override func viewDidLoad() {
        self.view.addSubview(self.topMenuView)
        self.topMenuView.snp.makeConstraints { maker in
            maker.left.right.top.equalTo(self.view)
            maker.height.equalTo(48)
        }
        self.topMenuView.requestViewData()
    }
```

### 后续
经过多次使用总结: 上面通过约束自适应的方案当只有一个 cell 的时候,会居中,需要自定义 followLayout 设置左对齐,总之会有各种各样的问题,经老开发提醒,UICollectionView 的宽高最好在 外部给定, 即 sizeForItemAtIndexPath 这里返回固定的宽高,然后 不要设置 estimatedItemSize,然后在 自定义Cell里面通过约束布局,
注意 UICollectionView 有默认的 spacing 间距,需要设置为0,才不会出现计算之外的问题,
```
        UICollectionViewFlowLayout *layout = [UICollectionViewFlowLayout new];
        layout.minimumInteritemSpacing = 0;
        layout.minimumLineSpacing = 0;
```

即趋势应该是  由外到内 的设置 大小.
