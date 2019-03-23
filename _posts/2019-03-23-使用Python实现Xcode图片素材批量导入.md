#1 Xcode图片批量导入

使用Xcode开发过程中，常常会使用到图片素材作为App图标、背景图片、页面等等。虽然Xcode提供了将图片拖入Image.xcassets的可视化操作，但是当需要批量将图片素材（比如100张以上）导入Xcode时，就变成一个非常繁琐的体力活了。打开Xcode，新建图片集Imageset，为图片集重命名，然后打开Finder，拖入需要的图片。
如果只是零散的几个图片还好，若是要重复100次的工作的话未免太过繁杂。就好像小学生的抄写作业一样，每个生字抄写20遍什么的，如果那时有计算机的话，我会写一个循环然后print就好。

#2 图片批量导入解决方案

那么问题来了，如何才能快速地一键导入大量图片呢？首先观察了一下Xcode的Imageset文件结构，发现是由图片文件和一个Contents.json组成，存放在一个目录内。
比如Imageset名称为P99的话，打开Finder可以看到
在P99.imageset目录下
有P99.jpeg和Contents.json
而这个Contents.json里面是什么内容呢，不妨打开看一下

```
{
  "images": [
    {
      "idiom": "universal",
      "scale": "1x",
      "filename": "P99.jpeg"
    },
    {
      "idiom": "universal",
      "scale": "2x"
    },
    {
      "idiom": "universal",
      "scale": "3x"
    }
  ],
  "info": {
    "version": 1,
    "author": "xcode"
  }
}
```
其实内容也非常清楚，标识了图片文件名，1x 2x 3x对应不同的图片比例，这里只放了1张图片所以2x 3x留空即可。如果想配上所有比例的图片也可以补充2x 3x对应的图片名。
那么其实只要读取所有需要拷贝的图片名，然后自动生成对应的Contents.json和imageset目录，接着打开Finder把目录复制到Xcode对应的图片存放位置就可以了。
保险起见，试了一下手工改了一个P98的imageset目录，然后放入Xcode，发现可以立即识别，那接下来就可以动手开始写Python了。

#3 使用Python实现Xcode图片素材的批量生成

要说写工具的话没有比Python更合适的了，因为可以几行代码解决很多问题。简要的分析一下，主要做的事情分为几步
1.读取需要复制的图片文件信息
2.根据图片文件名自动生成imageset目录和Contents.json

代码如下
```
# -*- coding:utf-8 -*- 
'''
自动生成 Xcode的 imageasset
生成后直接复制到Xcode  imageasset 目录即可
'''
import os, glob
import json

#默认输入的图片存放在当前路径input目录下
path = "input"
#默认输出位置存放在当前路径output目录下
output_path = "output"
imgslist = glob.glob(path+'/*.*')

#创建imageaset目录与Contents.json  
def create_img_dir():
	for imgs in imgslist:
		#split分解为路径+文件名
		imgspath, filename = os.path.split(imgs)
		#splitext分解为文件+后缀名
		name, ext = os.path.splitext(filename)
		print name
		#在输出目录下，创建imageset目录
		out_imageset =  output_path+"/"+ name + ".imageset"
		if(not os.path.exists(out_imageset)):
			os.mkdir(out_imageset)
			
			
		#data为python对象  filename为读取到的图片文件名，此处只配置1x大小的图片
		#如果没有指定 2x 3x 则留空
		data = {
			"images" : [
				{
				  "idiom" : "universal",
				  "filename" : filename,
				  "scale" : "1x"
				},
				{
				  "idiom" : "universal",
				  "scale" : "2x"
				},
				{
				  "idiom" : "universal",
				  "scale" : "3x"
				}
			  ],
			  "info" : {
				"version" : 1,
				"author" : "xcode"
			  }
			}
		print 'DATA:', repr(data)
		#dumps   python 对象转换为 json string
		data_string = json.dumps(data)
		#print 'JSON:', data_string
		
		#原图片文件复制到 输出目录out_imageset下 
		open(out_imageset + "/" + filename, "wb").write(open(imgspath +"/"+ filename, "rb").read())
		
		#将python对象直接通过 dump方法  输出json到文件
		with open(out_imageset + "/" + "Contents.json",'w') as fp:
			json.dump(data,fp)


if __name__ == '__main__':
	create_img_dir()
	print "done"
```
Python脚本使用方法：
1 把需要导入的图片放入input目录中 
2 运行 python img_json_export.py 
3 在Finder中把所有的output目录下的文件复制到Xcode imageset对应的目录下，即可在Xcode中查询到所有的图片已经导入成功

完整的代码已经提交到github上，可供参考 https://github.com/bear0830/XcodeImgImport
