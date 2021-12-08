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
