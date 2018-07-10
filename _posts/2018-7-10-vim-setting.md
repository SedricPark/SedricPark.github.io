---
categories: vim
---

### VIM 8 설치 관련

#### ubuntu 16.04 LTS, VIM 8 설치

* 우분투에서 apt-get으로 vim을 설치시 7버전이 설치 되는데, vim 8로 업그레이드
* vim 8 install 방법

```python
sudo add-apt-repository ppa:jonathonf/vim
sudo apt update
sudo apt install vim

# vim 버전확인
vim --version
```

* vim 8 uninstall

```pyehon
sudo apt remove vim
sudo add-apt-repository --remove ppa:jonathonf/vim
```

#### MacOS MacVim 설치하기

* brew을 이용해서 설치하기
  * GUI macvim 가 아니라 터미널에서 vim을 사용하기 위함
  * `brew install macvim`
* macvim 사용하기
  * `mvim -v`
  * -v 옵션을 주는 이유는 이 옵션을 주지 않으면 맥에 기본으로 설치된 vi 7 버전으로 실행 됨
* alias 설정해주기
  * alias를 설정해주는 이유는, 매번 mvim - v를 타이핑 해야 하는것을 줄이기 위해
  * zsh를 사용하기 때문에 아래 코드는 zsh 기준

```python
# zshrc를 열어서 alias 추가하기
vi ~/.zshrc

# 아래 쪽에 alias 추가하고 저장
alias vim='mvim -v'

# zshrc 적용
source ~/.zshrc
```

* 아래와 같은 터미널 에러 발생시 처리

```python
dyld: Library not loaded: /usr/local/lib/libgdbm.4.dylib  
  Referenced from: /usr/local/bin/zsh  
  Reason: image not found
    
# 아래와 같이 zsh을과 링크를 다시 걸어준다
brew reinstall zsh && brew unlink zsh && brew link zsh
```



#### VIM 플러그인 설치 및 플러그인 관리

* vim을 에디터로 사용하기 위해 좋은 플로그인을 구해야 하고, 플러그인을 관리하기 위한 툴이 필요
* vim에서 플러그인 관리툴로 Vundle을 선택
* Vundle 설치 : git을 이용해서 github를  내 컴퓨터 ~/.vim/bundle/Vundle.vim에 설치
  * `git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim`
  * Vundled은 `~/.vimrc`에서 플러그인을 관리 함
* `~/.vimrc`에서 아래 내용 작성

```python
set nocompatible              " be iMproved, required
filetype off                  " required

set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()

Plugin 'VundleVim/Vundle.vim'

call vundle#end()            " required
filetype plugin indent on    " required

# call vundle#begin()과 call vundle#end() 사이에 사용할 플러그를 작성
# 위의 예시에는 Plugin 'VundleVim/Vundle.vim'을 작성 함
```

* 플러그인  설치하는 방법(PluginSearch를 통해) 및 삭제 방법

```python
# ~/.vimrc를 열어서 명령어 모드에서
PluginSearch # 입력

# Plugin 목록들이 왼쪽에 나열
# 키보드 's'를 눌러 플로그인 검색이 가능 함
# 원하는 플로그인을 선택 후에
# 키보드 'yy'를 눌러서 플러그인 복사
# ctral + w + w 눌러스 우측 화면 이동
# 키보드 p를 눌러서 우측 화면 복사
# wq로 저장한 다음에
# 아래 명령어로 플러그인 설치
PluginInstall
    
# uninstall은 플러그 내용을 지우고 아래 명령어 실행
:PluginClean
```

* 설치된 플러그인 사용 방법 : The-NERD-tree 기준
  * https://vimawesome.com/plugin/nerdtree-red

```python
# NERD tree 사용
:NERDTree # 명령창에 입력
# m을 눌러 파일 삭제 이동 등등 가능

# NERDTree 단축키 사용 하기
~/.vimrc # 를 열어서 아래 내용 추가
map <Leader>n <ESC>:NERDTree<CR> # 단축키늘 \n로 설정, 명령모드에서 \+n 누름 이동
```

* vim-airline 세팅
  * https://github.com/vim-airline/vim-airline

```python
# airline 및 테마 설치
Plugin 'vim-airline/vim-airline'
Plugin 'vim-airline/vim-airline-themes'

# airline 상단 탭 설정
let g:airline#extensions#tabline#enabled = 1
    
# 상단 탭 테마, 테마 종류
let g:airline#extensions#tabline#formatter = 'uniuqe_tail'
    
# 상단 탭 이동 단축키
# 기본은 :bn, :bp
map <leader>q :bp<CR> # \ + q
map <leader>w :bn<CR> # \ + w

# air라인 하단바 테마 세팅
# 테마에 따라 별도 플러그인 세팅이 필요 할 수 도 있음
# 아래는 별로 플러그인 필요
Plugin 'bling/vim-bufferline'
let g:airline_theme='badwolf'
    
# Powerline 미적용을 하단 라인 모양 등이 예쁘게? 제대로 안나올 때
# powerline을 지원하는 폰트를 설치하고, iterm이나 터미널 폰트에 powerline 폰트 지정
# powrline 폰트 지원 다운로드 받기 및 폰트 설치하기
https://github.com/powerline/fonts
# 폰트는 폰트명 + powerline 으로 구글링하면 많이 나옴
    
# iterm2 나 터미널에 폰트 적용
# ~/.vimrc에 아래내용 추가
let g:airline_powerline_fonts = 1
```

* 각종 유용한 플러그인

```python
# 코드 변경 내역 한눈에 보기, vim-airline과 연동
Plugin "airblade/vim-gitutter"

# vim에서 git 사용하기, airline에 브랜치명이 나옴
Plugin "tpope/vim-fugitive"

# 코드 문법 체크, 문법 오류시 airline 하단에 오류명 확인
Plugin 'Syntastic'

# 파일 찾기, 검색시 인덱스로 오류도 자주나고 렉도 걸림 비추
# 검색 인덱스 제외 세팅해주어야 함
Plugin 'ctrlp.vim'
# 검색 인덱스 제외 세팅
let g:ctrlp_custom_ignore = {
  \ 'dir':  '\.git$\|public$\|log$\|tmp$\|vendor$',
  \ 'file': '\v\.(exe|so|dll)$'
\ }
    
# 컬러 테마 변경
# https://github.com/KeitaNakamura/neodark.vim
Plugin 'KeitaNakamura/neodark.vim'
# 아래 세팅 추가
colorscheme neodark
let g:neodark#background = '#202020'
    
# vim true color 지원
set termguicolors

# 라인 넘버 세팅
set number
```

* 최종 세팅 최종본

```python
set nocompatible
filetype off

set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()

Plugin 'VundleVim/Vundle.vim'
Plugin 'The-NERD-tree'
Plugin 'vim-airline/vim-airline'
Plugin 'vim-airline/vim-airline-themes'
Plugin 'bling/vim-bufferline'
Plugin 'airblade/vim-gitgutter'
Plugin 'tpope/vim-fugitive'
Plugin 'Syntastic'
Plugin 'ctrlp.vim'
Plugin 'KeitaNakamura/neodark.vim'

filetype plugin indent on
call vundle#end()

map <Leader>q :bp<CR>
map <Leader>w :bn<CR>
map <Leader>t <ESC>:NERDTree<CR>

let g:airline_theme='simple'
let g:airline#extensions#tabline#enabled = 1
let g:airline#extensions#tabline#formatter = 'unique_tail'

let g:airline_powerline_fonts = 1

# https://github.com/KeitaNakamura/neodark.vim
colorscheme neodark
let g:neodark#background = '#202020'


let g:ctrlp_custom_ignore = {
  \ 'dir':  '\.git$\|public$\|log$\|tmp$\|vendor$',
  \ 'file': '\v\.(exe|so|dll)$'
\ }

syntax on
set termguicolors
set number
set tabstop=8
set expandtab
set softtabstop=4
set shiftwidth=4
filetype indent on
```

### Oh My Zsh 테마 설정

* oh my zsh은 커맨드 라인의 색이나 기타를 변경하는 테마
* 기본적으로 `~/.oh-my-zsh/themes` 에 있는 테마를 `~/.zshrc`에 등록해주면 된다

```python
# ~/.zshrc
ZSH_THEME="테마이름"  # ~/.oh-my-zsh/themes에 있는 테마 이름 작성
```

* darcula 테마는 없기 때문에 홈페이지에서 다운로드 받고 설정
  * https://draculatheme.com/zsh/

### Oh My Zsh 플러그인

* syntax-highlighting and autosuggestions 설치
  * 참고 : http://www.ivaylopavlov.com/setting-oh-zsh-zsh/
* autosuggestions는 경로나 명령어 자동추천

```python
#git clone으로  하는 방법
# autosuggestions 설치하기
git clone https://github.com/zsh-users/zsh-autosuggestions ~/.zsh/zsh-autosuggestions
    
# ~/.zshrc 아래 내용 작성하고 적용시키기
source ~/.zsh/zsh-autosuggestions/zsh-autosuggestions.zsh
source ~/.zshrc
# 터미널 재시작

#oh-my-zsh 이용은 아래 사이트 참고
https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md
    
# 삭제 방법 
rm -rf ~/.zsh/zsh-autosuggestions
```

* syntax-highlighting는 명령어 강조

```python
# 설치 가이드
https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md
    
# git clone으로 설치
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.zsh/zsh-syntax-highlighting

# ~/.zshrc 에 아래내용 추가
source ~/.zsh/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

