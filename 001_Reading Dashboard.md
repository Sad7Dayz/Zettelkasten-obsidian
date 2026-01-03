# 📊 Reading Dashboard

> 📌 Dataview 플러그인 필요  
> 📁 대상 폴더: `01 Literature Notes`

---

## 📚 전체 독서 현황
```dataview
TABLE
  status AS "상태",
  author AS "저자",
  rating AS "⭐",
  reread AS "재독",
  created AS "작성일"
FROM "01 Literature Notes"
WHERE contains(tags, "literature-note")
SORT created DESC
```

## 📖 읽는 중
```dataview
TABLE
  author AS "저자",
  created AS "시작일"
FROM "01 Literature Notes"
WHERE status = "📚 reading"
SORT created ASC
```

## ✅ 읽기 완료
```dataview
TABLE
  author AS "저자",
  rating AS "⭐",
  created AS "완료일"
FROM "01 Literature Notes"
WHERE status = "✅ finished"
SORT created DESC

```

## ⏸️ 보류 중
```dataview
LIST
FROM "01 Literature Notes"
WHERE status = "⏸️ paused"

```

## 🔢 상태별 권수
```dataview
TABLE
  length(rows) AS "권수"
FROM "01 Literature Notes"
GROUP BY status
```

## 🗓️ 월별 독서 완료 기록
```dataview
TABLE
  length(rows) AS "읽은 수"
FROM "01 Literature Notes"
WHERE status = "✅ finished"
GROUP BY dateformat(created, "yyyy-MM") AS "월"
SORT "월" DESC

```

## 🧠 Permanent Note 연결 현황
```dataview
TABLE
  file.link AS "Literature Note",
  length(file.outlinks) AS "연결 수"
FROM "01 Literature Notes"
SORT length(file.outlinks) DESC

```