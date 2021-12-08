# The ThinkPHP6 Image Package

基于ThinkPHP官方的topthink/think-image改进而来，修复部分bug,移除了不支持的gif解码，统一jpeg/jpg和png的统一操作体验,后期准备接入intervention/image库，目前使用方法同topthink/think-image。

> think-image和intervention/image:前者是ThinkPHP出品，后者是Laravel采用的图像处理，后面功能更强大，支持GD和Imagick。不过二者对gif支持都不好，虽然Think-image集成gif编解码，但实测效果不好，而后者把gif当前静态图片，若想要支持则需要intervention/gif库，不过该库也是在更新中，功能还不全面。

> 升级计划
> 1. 优化原版think-image代码，修复部分bug，统一了jpg和png保存时质量的含义。如不能识别gif问题，虽然将编解码修改成支持PHP8，但输出效果并不理想，关闭了gif的支持【完成】
> 2. 接入intervention/image,提供是使用原生还是基于intervention/image(可选择GD或Imagick)处理图片，同样不支持gif【待完成】
> 3. 接入intervention/gif,提供对gif的支持，经测试目前intervention/gif和网上流传的gif编解码对gif处理不理想，这个可能要等intervention/gif支持再完成【待完成】


## 安装

> composer require woxiaoyao81/think-image

## 使用

### 基本使用
支持文件或Http的请求文件
$image = \think\Image::open('./image.jpg');
或者
$image = \think\Image::open(request()->file('image'));
$image->crop(...)
    ->thumb(...)
    ->water(...)
    ->text(....)
    ->save(..);

### 具体使用
> 使用open方法打开图像文件进行相关操作
```php
$image = \think\Image::open('./image.png');
也可以从直接获取当前请求中的文件上传对象
$image = \think\Image::open(request()->file('image'));
获取图像信息
$image = \think\Image::open('./image.png');
// 返回图片的宽度
$width = $image->width(); 
// 返回图片的高度
$height = $image->height(); 
// 返回图片的类型
$type = $image->type(); 
// 返回图片的mime类型
$mime = $image->mime(); 
// 返回图片的尺寸数组 0 图片宽度 1 图片高度
$size = $image->size(); 
```
> 使用crop和save方法完成裁剪图片功能
```php
$image = \think\Image::open('./image.png');
// 将图片裁剪为300x300并保存为crop.png
$image->crop(300, 300)->save('./crop.png');
// 支持从某个坐标开始裁剪，例如下面从（100，30）开始裁剪
$image->crop(300, 300,100,30)->save('./crop.png');
```
> 使用thumb方法生成缩略图
```php
$image = \think\Image::open('./image.png');
// 按照原图的比例生成一个最大为150*150的缩略图并保存为thumb.png
$image->thumb(150, 150)->save('./thumb.png');
// 居中裁剪
$image->thumb(150,150, \think\Image::THUMB_CENTER)->save('./thumb.png');
// 右下角剪裁
$image->thumb(150,150,\think\Image::THUMB_SOUTHEAST)->save('./thumb.png');
```
> 使用flip可以对图像进行翻转操作，默认是以x轴进行翻转
```php
$image = \think\Image::open('./image.png');
// 对图像进行以x轴进行翻转操作
$image->flip()->save('./filp_image.png');
// 对图像进行以y轴进行翻转操作
$image->flip(\think\image::FLIP_Y)->save('./filp_image.png');
```
> 使用rotate可以对图像进行旋转操作（默认是顺时针旋转90度）
```php
$image = \think\Image::open('./image.png');
// 对图像使用默认的顺时针旋转90度操作
$image->rotate()->save('./rotate_image.png');
```
> 系统支持添加图片及文字水印
```php
$image = \think\Image::open('./image.png');
// 给原图左上角添加水印并保存water_image.png
$image->water('./logo.png')->save('water_image.png'); 
// 给原图左上角添加水印并保存water_image.png
$image->water('./logo.png', \think\Image::WATER_NORTHWEST)->save('water_image.png');
// 给原图左上角添加透明度为50的水印并保存alpha_image.png
$image->water('./logo.png', \think\Image::WATER_NORTHWEST, 50)->save('alpha_image.png');
// 给图片添加文字水印（我们复制一个字体文件HYQingKongTiJ.ttf到入口目录）生成一个像素20px，颜色为#ffffff的水印效果
$image->text('为API开发设计的高性能框架', 'HYQingKongTiJ.ttf', 20, '#ffffff')->save('text_image.png');
```
