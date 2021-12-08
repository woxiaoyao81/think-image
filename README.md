# The ThinkPHP6 Image Package

基于ThinkPHP官方的topthink/think-image改进而来，修复部分bug,移除了不支持的gif解码，统一jpeg/jpg和png的统一操作体验,后期准备接入intervention/image库，目前使用方法同topthink/think-image。

> think-image和intervention/image:前者是ThinkPHP出品，后者是Laravel采用的图像处理，后面功能更强大，支持GD和Imagick。不过二者对gif支持都不好，虽然Think-image集成gif编解码，但实测效果不好，而后者把gif当前静态图片，若想要支持则需要intervention/gif库，不过该库也是在更新中，功能还不全面。


## 安装

> composer require woxiaoyao81/think-image

## 使用

~~~
$image = \think\Image::open('./image.jpg');
或者
$image = \think\Image::open(request()->file('image'));


$image->crop(...)
    ->thumb(...)
    ->water(...)
    ->text(....)
    ->save(..);

~~~