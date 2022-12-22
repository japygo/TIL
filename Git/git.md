# Git
> 간단한 깃 사용법

---

## 저장소 만들기
```git
git init
```

## 폴더/파일 올리기
```git
# 폴더 전체
git add .

# 파일 추가
git add <파일명>

# 커밋했던 파일들만 추가
git add -u
```

## 커밋하기
```git
git commit

# 커밋메시지 추가
git commit -m "커밋메시지"
```

## 변경된 내용 확인
```git
git status
```

## 원격 저장소 복제하기
```git
git clone <원격 저장소 주소>
```

## 원격 저장소 등록하기
```git
git remote add <원격 저장소 이름> <원격 저장소 주소>
```

## 원격 저장소 반영하기
```git
git push <원격 저장소 이름> <브랜치 이름>
```

## 원격 저장소 가져오기
```git
git pull
```

---

참조 사이트

[누구나 쉽게 이해할수 있는 Git 입문](https://backlog.com/git-tutorial/kr/)
