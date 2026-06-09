---
title: 첫 번째 블로그 포스팅
date: 2026-06-09 15:55:00 +0900
categories: [개발, Python]
tags: [Python]
---
# pip

pip는 파이썬을 설치하면 포함되어 있는 툴로 패키지를 설치/제거하기 위해 기본적으로 활용하며, 보통 아래의 세 가지 구문이 제일 많이 사용될 것이다.

```PowerShell
> pip install <package_name1> <package_name2> ...    # 설치
> pip uninstall <package_name1> <package_name2> ...  # 제거
> pip list                                           # 설치된 패키지 목록
```
그러면 패키지는 다운 후 `<python_path>/Lib/site-packages/` 경로에 설치되어 파이썬 코드 개발에 활용할 수 있게 된다.

## 1. 사용 전 기본적으로 알아야 할 사항

- pip는 형태가 어떻든 간에 `<package_name>.whl` 이라는 wheel 파일을 다운 받아서 설치한다.
- pip는 따로 지정하지 않으면 PyPI라는 저장소(repo)에서 패키지를 찾는다.
- URL을 지정하면 PyPI 외의 저장소에서도 `.whl` 파일을 다운 받을 수 있다.
- 어떤 패키지를 다운받을 때 버전을 특정 버전을 지정할 수 있다.

## 2. 버전 지정 설치

💡 *어떤 버전이 존재하는 지 확인하기 위해서는 PyPI (https://pypi.org)에 접속해서 검색을 통해 확인할 수 밖에 없다.*

## 3. 다양한 패키지 설치법

파이썬에서 어떤 패키지를 다운받아 설치하는 법은 생각보다 다양하고, 파이썬을 다룰 때 기본적으로 잘 숙지해야 하는 부분이다. 

### 3.1 PyPI에서 whl 파일을 다운받아 설치

제일 기본적으로 사용하는 방식이다.

```PowerShell
> pip install <package_name>
```

💡 *이 방식을 사용할 때 항상 "pip가 패키지를 설치한다"라고 생각하지 말고, "pip가 내 파이썬에 맞는 .whl 파일을 찾아서 whl 파일을 다운로드 한 후 설치한다"라고 생각하자.*

💡 *본인이 개발한 코드로 직접 패키지 .whl 파일을 만들 수도 있다. 이 때 setuptools, wheel, build의 세 개의 패키지가 필요하다. 직접 만든 패키지를 PyPI에 업로드하여 다른 사람들과 공유하는 것도 매우 쉽다.*

### 3.2 whl 파일을 다운받아 설치

모든 설치의 기본이 `.whl` 파일이라는 것을 이해하고 나면 PyPI에 찾고 있는 패키지가 없더라도 `.whl` 파일을 다른데서 구해서 설치가 가능하다. 이 때는 `<package_name>`이 아니라 `<package_file_name>.whl`로 파일 명을 직접 지정하면 된다.

```PowerShell
> pip install <package_file_name>.whl
```

### 3.3 PyPI 외의 패키지 저장소에서 다운받아 설치

PyPI가 일반적으로 파이썬에서 .whl을 다운받기 위한 기본 레포이지만, 이외의 레포를 통해 패키지를 다운받을 수도 있으며, 사설 레포를 운영할 수 있다. 특히 CUDA를 이용하는 경우에는 PyPI 외의 repo를 활용해야 하는 경우가 많다.

인공지능 개발에서 많이 활용하는 PyTorch의 경우 PyPI에는 CPU 버전밖에 없으며, CUDA 버전을 구하기 위해서는 아래와 같이 다른 레포를 --index-url로 지정해 주어야 한다.

```PowerShell
> pip install torch --index-url https://download.pytorch.org/whl/cu121
```

참고로 --index-url과 비슷하게 --extra-index-url이라는 옵션도 있다. --extra-index-url은 먼저 PyPI를 찾고, PyPI에서 발견하지 못할 경우 -extra-index-url에 지정한 레포를 사용하라는 지정이다.

### 3.4 소스 코드를 다운 받아 빌드 후 설치 (1)

간혹 `.whl`이 아니라 `.tar.gz` 등의 형식으로 소스코드를 압축한 형태로 패키지가 제공되는 경우가 있다. 이 때는 모든 압축 파일이 이런 형태로 설치되는 것이 아니라, 내부에 `pyproject.toml`이나 `setup.py`와 같은 설정 파일이 포함되어 있는 압축 파일에 한정하며, 해당 소스코드를 빌드할 수 있는 빌드 툴(비주얼 스튜디오 등, 소스 코드의 종류에 따라 달라진다)이 존재해야 한다. 여기서도 역시 내부적으로 `.whl` 파일을 생성 후 `.whl` 파일을 설치하는 과정을 거친다.

```powershell
> pip install <source_code>.tar.gz
```

`tar.gz`을 설치하는 것과 동일하게, 아래와 같이 아예 압축을 푼 소스 코드 폴더 형식으로도 제공 가능하다.

```powershell
> pip install <source_code_folder>
```

### 3.5 소스 코드를 다운 받아 빌드 후 설치 (2)

GitHub 등의 repo에 패키지의 소스 코드가 존재할 경우 막바로 인터넷 주소를 사용해서 다운받아 설치도 가능하다.

```powershell
> pip install git+<github_url>
> pip install git+https://github.com/user/repo.git # example
```

내부적으로 실제 동작은 GitHub에서 패키지 레포 폴더를 통째로 복사(clone)해 온 후에 빌드해서 `.whl`을 만들고 설치 후 복사해 온 폴더를 삭제하는 방식이라 위에서 기술한 방식과 동일하다고 볼 수 있다.

### 3.6 소스 코드를 다운 받아 빌드 후 설치 (3)

실제 개발자들은 GitHub의 패키지 레포를 복사해 온 후에 해당 레포(즉 패키지 폴더)를 내 컴퓨터에 유지하는 경우가 많다. 이것은 (1) 해당 패키지 소스 코드를 뒤져가며 분석해야 할 경우가 종종 발생하고 (2) 내가 원 패키지의 소스 코드를 변경해서 사용해야 할 경우도 종종 발생하기 때문이다.
  이 때는 위에 언급한 첫 번째 방식(`pip install`로 폴더를 설치)으로 설치도 가능하지만, 아래와 같이 `-e` 옵션으로 포함해서 “개발 모드”로 설치하는 경우가 흔하다.

```powershell
> pip install -e <source_code_folder>
```
이 -e 옵션을 사용하지 않을 경우는 `.whl` 파일을 만들고

`<python_folder>/Lib/site-packages/<package_name>`

에 설치할 패키지 파일들이 복사된다. 하지만, `-e` 옵션을 사용하게 되면 `site-packages/<package_name>` 폴더에 파일들을 복사하지 않고 원래 소스 코드 폴더의 내용을 설치된 패키지로 활용한다. 이 옵션은 특히 내가 해당 패키지의 내용을 바꿔도 그대로 그 패키지를 활용하는 내 소스 코드에 막바로 반영된다는 장점이 있다. (즉, 해당 패키지를 내 소스 코드처럼 활용이 가능하다.)

### 3.7 소스 코드를 다운 받아 빌드 후 설치 (4)

(GitHub 등에서) 다운받은 소스 코드를 다운받아 빌드해서 사용할 경우에는 설치에 문제가 간혹 발생한다. 이 때 --no-deps나 --no-build-isolation 등의 옵션을 추가로 이용해야 할 경우도 있으므로 혹시라도 문제가 생기면 이런 추가 옵션에 대해 찾아보기 바란다.
