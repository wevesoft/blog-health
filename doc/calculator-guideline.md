# 계산기 추가 지침서

새 성분 계산기를 만들 때 참고할 구조·필수 요소·검증 절차를 정리한 문서입니다.

---

## 1. 필수 구조 (비타민 D 계산기 기준)

### 1.1 HTML 구조

- **헤더**: 제목 + 한 줄 설명
- **입력 섹션**: `section-title` + `field`(label + select/input/checkbox-group)
- **버튼**: `calc-btn` — `onclick="calculateXXX()"`
- **결과 섹션** (`result-section`, 처음엔 `display:none`):
  1. **적용된 조건** (`result-applied`): `appliedList`(ul), `appliedEffect`(div)
  2. **권장량 카드** (`result-card primary`): `resultMain`, "입력하신 조건에 따라 계산된 맞춤 권장량이에요.", `resultRange`
  3. **추천 형태** (`result-card secondary`): `resultType`
  4. **섭취 방법** (`result-card secondary`): `resultSplit`
  5. **참고 팁** (`result-card secondary`): `resultTips` (ul)
  6. **계산 근거** (`calc-basis` details): `calcExplanation`
- **참고 문헌 블록** (`trust-block`): 참고 문헌 링크 + **보정 문구** + 면책

### 1.2 CSS 필수

- **애니메이션**: `@keyframes fadeInUp`, `fadeIn`  
- **결과 표시**: `.result-section.visible` + `.result-applied`, `.result-card`, `.calc-basis` 의 `animation`/`animation-delay`  
- **적용된 조건 카드**: `.result-applied` (녹색 계열 배경·테두리), `.result-applied .effect`  
- **초기 상태**: 결과 영역 자식들은 `opacity: 0` (애니메이션 both로 등장)

### 1.3 JS 필수

1. **계산 함수**  
   - 입력값 읽기 → 기본 RDA + 보정 → `total` 계산 → 상한/하한 적용  
   - **적용된 조건**: `appliedList`에 선택한 값 라벨 목록 (예: 연령, 성별, 목적)  
   - **효과 문장**: `appliedEffect`에 "→ 조건 A를 적용했어요. 조건 B를 반영해 가산했어요." 형태  
   - 결과 텍스트 채우기 (`resultMain`, `resultRange`, `resultType`, `resultSplit`, `resultTips`, `calcExplanation`)  
   - **재계산 시 애니메이션 재생**: `resultSection.classList.remove('visible')` → `void resultSection.offsetHeight` → `resultSection.classList.add('visible')` → `setTimeout(..., 100)` 후 `scrollIntoView`

2. **슬라이더/프리셋** (체중 등 사용 시)  
   - input[range]와 input[number], preset 버튼 연동

---

## 2. 참고 문헌 및 신뢰도

### 2.1 반드시 넣을 내용

- **참고 문헌**: NIH ODS 해당 성분 Fact Sheet 링크 (또는 AHA 등 공식 가이드)  
- **한국영양학회**: `https://www.kns.or.kr/nutrient.asp` (해당 성분 언급)  
- **보정 문구**:  
  "기본 권장량은 (NIH ODS 등) RDA를 참고했고, **(해당 성분별 보정 항목) 보정은 문헌에 명시된 공식이 아니라 단순화된 참고 모델입니다.**"  
- **면책**: "본 계산기는 참고용이며, 진단·치료·처방을 대체하지 않습니다. 복용 전 의료진 상담을 권장합니다."

### 2.2 검증

- 새 계산기 추가 후 `doc/calculator-references-verification.md` 에 한 줄 추가:  
  기본 RDA·상한 출처, 보정은 "단순화 모델"임을 명시  
- RDA 수치는 NIH ODS 해당 Fact Sheet Table에서 **직접 확인** 후 반영

---

## 3. 파일·경로 규칙

- **파일명**: `calc/{성분영문}_calc.html` (예: `calcium_calc.html`, `zinc_calc.html`)  
- **doc 매핑**: `doc/ingredient-calc-map.md` 에 성분명, 계산기 경로, 블로그 경로 추가  
- **블로그**: `doc/blog/{성분영문}.md` 에 해당 성분 글 + 계산기 링크

---

## 4. 체크리스트 (새 계산기 추가 시)

- [ ] NIH ODS(또는 공식 가이드) 해당 성분 RDA·UL 확인
- [ ] 기본값·보정 로직 주석에 출처 명시 (예: `/* NIH ODS Table 1: ... */`)
- [ ] 결과 영역에 **적용된 조건** 카드 + **효과 문장** 포함
- [ ] 권장량 아래 "입력하신 조건에 따라 계산된 맞춤 권장량이에요." 문구
- [ ] 참고 문헌 블록에 **보정은 단순화된 참고 모델** 문구 + 면책
- [ ] 재계산 시 결과 영역 애니메이션 재생
- [ ] `ingredient-calc-map.md` 갱신
- [ ] `calculator-references-verification.md` 에 검증 결과 한 줄 추가

---

## 5. 참고할 기존 계산기

| 성분 | 파일 | 비고 |
|------|------|------|
| 비타민 D | `calc/vitamin-d_calc.html` | 적용된 조건·효과·애니메이션·보정 문구 완비 |
| 마그네슘 | `calc/magnesume_calc.html` | 슬라이더·프리셋·증상 체크 |
| 철분 | `calc/iron_calc.html` | 성별·연령·임신·수유·증상 |
| 오메가3 | `calc/omega3_calc.html` | 체중·목적·생선 섭취 |

새 계산기는 **비타민 D** 구조를 복사한 뒤, 입력 항목·RDA·보정 로직·적용된 조건/효과 문장만 해당 성분에 맞게 바꾸면 됩니다.
