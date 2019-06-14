---
layout: post
title: "[Python] 아나콘다 설치방법, 주요 명령어 정리"
date:   2019-02-01
categories: Python
---

Python 배포판 아나콘다의 설치 방법과 자주 사용하는 명령어를 정리한 글입니다.

## 아나콘다란 무엇인가
Continuum Analytics에서 제작한 파이썬 배포판이다. 수학, 과학 등과 관련된 다양한 파이썬 패키지를 포함하고 있으며, 상업용으로도 무료 사용이 가능하다. **패키지 의존성을 관리**해주기 때문에, 가상환경에 따라 독립적으로 패키지 관리를 쉽게 할 수 있다는 장점을 가지고 있다.

>현재 아나콘다 홈페이지에는 최신 파이썬 버전인 python 3.7 버전의 설치 파일을 제공하고 있다. 다른 버전의 파이썬을 깔고 싶다면, 설치한 후에 다음 명령어를 실행하면 된다. [[공식홈페이지 링크]](http://docs.anaconda.com/anaconda/user-guide/faq/#how-do-i-get-the-latest-anaconda-with-python-3-5)

	conda install python={version}


## 새 가상환경 생성 (w. anaconda)

	conda create -n "{venv_name}" python=3 anaconda

anaconda를 같이 설치해주어야 jupyter notebook을 사용할 때, 가상환경의 파이썬을 사용할 수 있다.

### 가상환경 삭제
	conda remote -n "{venv_name}" --all 

### 가상환경 활성화
	source activate {venv_name}

### 가상환경 비활성화
	source deactivate

### 가상환경 목록 확인
	conda info --envs

### 생성한 가상환경에 패키지 추가 설치
	conda install -n "{venv_name}" {packages..}
	