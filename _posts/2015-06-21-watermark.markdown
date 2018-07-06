---
layout: post
title:  "利用UIGraphics 截图，添加水印"
date:   2015-06-21 20:27:25 +0800
categories: jekyll update
---

#### 1.截图

```
- (UIImage*)imageWithUIView:(UIView*)view{
    UIGraphicsBeginImageContext(view.bounds.size);
    CGContextRef currnetContext = UIGraphicsGetCurrentContext();
    [view.layer renderInContext:currnetContext];
    // 从当前context中获取图片
    UIImage *image = UIGraphicsGetImageFromCurrentImageContext();
    // 使当前的context出堆栈
    UIGraphicsEndImageContext();
    return image;
}
```

```
/// swift
  func shortCut(view: UIView) -> UIImage {
        UIGraphicsBeginImageContext(view.bounds.size)
        let currContext = UIGraphicsGetCurrentContext()
        view.layer.render(in: currContext!)
        let image = UIGraphicsGetImageFromCurrentImageContext() as UIImage?
        UIGraphicsEndImageContext()
        
        return image!
    }

```
webview截图时，只能截图屏幕显示区域视图中显示的内容，所以需要依次把webview中的内容滚动到屏幕中做截图处理，可以先把当前正在显示的部分截图作为一个蒙层，在下面滚动webview截图，拼接到一起，完成之后回滚到原来的位置，蒙层消失，展示拼接的截图。

---

#### 2.添加水印
##### 添加文字水印
```
/// 添加文字水印
///
/// - Parameters:
///   - image: 要添加水印的图片
///   - text: 要添加的文字水印
///   - position: 文字水印的位置
///   - attributes: 文字水印的样式
/// - Returns: 添加了水印的图片
func addTextMark(image: UIImage, text: NSString, position: CGPoint, attributes: NSDictionary) -> UIImage {
    UIGraphicsBeginImageContextWithOptions(image.size, false, 0)
    image.draw(in: CGRect.init(x: 0, y: 0, width: image.size.width, height: image.size.height))
    text.draw(at: position, withAttributes: (attributes as! [NSAttributedStringKey : Any]))
    let newImg = UIGraphicsGetImageFromCurrentImageContext() as UIImage?
    UIGraphicsEndImageContext()

    return newImg!
}
```
##### 添加图片水印
```
/// 添加图片水印
///
/// - Parameters:
///   - image: 要添加水印的图片
///   - markImage: 水印图片
///   - position: 水印图片的位置
/// - Returns: 添加了水印的图片
func addImageMark(image: UIImage, markImage: UIImage, position: CGRect) -> UIImage {
    UIGraphicsBeginImageContextWithOptions(image.size, false, 0)
    image.draw(in: CGRect.init(x: 0, y: 0, width: image.size.width, height: image.size.height))
    markImage.draw(in: position)
    let newImage = UIGraphicsGetImageFromCurrentImageContext() as UIImage?
    UIGraphicsEndImageContext()

    return newImage!
}
```
