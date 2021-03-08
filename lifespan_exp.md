### 关于Dataset
https://github.com/royorel/FFHQ-Aging-Dataset.git
安装依赖后，用get_ffhq_aging.sh下载
首先通过download_ffhq_aging.py下载70000张1024x1024的头像，并裁剪成256x256，使用cpu
再用deeplab.py生成semantic map，使用gpu

### tnt facedata在哪
/data/scene_understanding/FFHQ-Aging/FFHQ-Aging-Dataset


### 流程
先下载ffhq-dataset-v2.json和LICIENCE.txt
parse json
specs=[70001个dict包含70000个in-the-wild-image的metadata和一个license]
specs打乱
查看已经下载好的specs
仍需要下载的图片在missing_specs里是个list
将list转化成队列
建立32个线程，同时不停从队列里取元素并下载改分辨率，存入256x256文件夹，并且删除in-the-wild-image
