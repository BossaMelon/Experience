### 开启gpu虚拟机流程
创建项目
申请gpu配额
创建虚拟机
在元数据加入ssh-keyls

### gcloud创建虚拟机
### marketplace模版
不能使用抢占型


### gcloud命令行工具
暂时使用gcloud compute


### 名词解释
- Compute Engine:可扩缩的高性能虚拟机
- Cloud Storage:对象存储
- image:映像，包含基本操作系统

### gcloud gpu比较(TFLOPS in FP32)
- A100:40G,19.5TFLOPS，3.1$/h
- T4:16G,8.1TFLOPS,0.35$/h
- V100:16G,15.7TFLOPS,2.48$/h
- P100:16G,9.3TFLOPS,1.46$/h
- P4:8G,5.5TFLOPS,0.6$/h
- K80:12G,4.37TFLOPS,0.45$/h



### 设置密码
sudo passwd

### 下载zsh
sudo apt-get install zsh

### 下载oh-my-zsh
sh -c "$(wget https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"

### 安装zsh-autosuggestions
cd ~/.oh-my-zsh/plugins
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
修改～/.zshrc
plugins=(zsh-autosuggestions)
插件之间换行不用加逗号

### 设置zsh为默认
sudo vi /etc/passwd
修改为/bin/zsh

### yuehao.zsh-theme
PROMPT='%{$fg_bold[cyan]%}%m %{$fg_bold[yellow]%}%~%{$reset_color%} $(git_prompt_info)'

ZSH_THEME_GIT_PROMPT_PREFIX="%{$fg_bold[blue]%}git:(%{$fg[red]%}"
ZSH_THEME_GIT_PROMPT_SUFFIX="%{$reset_color%} "
ZSH_THEME_GIT_PROMPT_DIRTY="%{$fg[blue]%}) %{$fg[yellow]%}✗"
ZSH_THEME_GIT_PROMPT_CLEAN="%{$fg[blue]%})"


### 下载miniconda
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh

### conda init
cat ~/.bashrc
复制conda相关


### 下载项目
git clone https://github.com/royorel/Lifespan_Age_Transformation_Synthesis.git
git clone https://github.com/AliaksandrSiarohin/first-order-model.git

### 安装依赖
conda create -n lifespan python=3.7
conda create -n fomm python=3.7
pip install -r requirements.txt
conda install pytorch torchvision torchaudio cudatoolkit=10.2 -c pytorchy
