[index.html](https://github.com/user-attachments/files/34913477/index.html)
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>웹소설 인기 랭킹</title>
<style>
:root { color-scheme: light; --bg:#f7f6f3; --card:#fff; --ink:#1f2328; --sub:#6b7280; --line:#e8e6e1;
  --kakao:#f5c518; --ridi:#1e9eff; --up:#d9534f; --down:#2d7ff9; }
*{box-sizing:border-box; margin:0}
body{font-family:'Apple SD Gothic Neo','Malgun Gothic','Noto Sans KR',sans-serif; background:var(--bg); color:var(--ink); padding:24px 16px 60px}
.wrap{max-width:860px; margin:0 auto}
header{text-align:center; margin-bottom:22px}
h1{font-size:1.65rem; letter-spacing:-.02em}
.date{color:var(--sub); font-size:.9rem; margin-top:6px}
.tabs{display:flex; gap:8px; justify-content:center; margin:20px 0 14px}
.tab{border:1px solid var(--line); background:var(--card); padding:9px 22px; border-radius:999px; font-size:.95rem; font-weight:600; cursor:pointer; color:var(--sub)}
.tab.active[data-p="kakao"]{background:var(--kakao); border-color:var(--kakao); color:#3b2f00}
.tab.active[data-p="ridi"]{background:var(--ridi); border-color:var(--ridi); color:#fff}
.search{display:block; width:100%; max-width:420px; margin:0 auto 18px; padding:10px 16px; border:1px solid var(--line); border-radius:10px; font-size:.95rem; background:var(--card)}
.card{background:var(--card); border:1px solid var(--line); border-radius:14px; overflow:hidden; box-shadow:0 1px 3px rgba(0,0,0,.04)}
table{width:100%; border-collapse:collapse; font-size:.93rem}
th{font-size:.78rem; color:var(--sub); font-weight:600; text-align:left; padding:12px 14px; border-bottom:1px solid var(--line); background:#fbfaf8}
td{padding:11px 14px; border-bottom:1px solid var(--line); vertical-align:middle}
tr:last-child td{border-bottom:none}
tr:hover td{background:#fafaf7}
.rank{font-weight:700; width:52px; text-align:center; font-variant-numeric:tabular-nums}
.top3{font-size:1.05rem}
.chg{width:58px; font-size:.78rem; white-space:nowrap; text-align:center; color:var(--sub)}
.chg .up{color:var(--up); font-weight:700}
.chg .down{color:var(--down); font-weight:700}
.chg .new{background:#eef6ee; color:#2f7d32; font-weight:700; padding:2px 7px; border-radius:6px; font-size:.72rem}
.title{font-weight:600; letter-spacing:-.01em}
.meta{color:var(--sub); font-size:.82rem; margin-top:2px}
.pub{color:var(--sub); font-size:.85rem; white-space:nowrap}
footer{text-align:center; color:var(--sub); font-size:.78rem; margin-top:26px; line-height:1.7}
@media (max-width:560px){ .pub{display:none} td,th{padding:10px 10px} }
</style>
</head>
<body>
<div class="wrap">
<header>
  <h1>📚 웹소설 인기 랭킹</h1>
  <div class="date" id="dateLine"></div>
</header>

<div class="tabs">
  <button class="tab active" data-p="kakao">카카오페이지</button>
  <button class="tab" data-p="ridi">리디</button>
</div>

<input class="search" id="q" type="search" placeholder="작품명·저자·출판사 검색">

<div class="card">
<table>
  <thead><tr><th style="text-align:center">순위</th><th style="text-align:center">변동</th><th>작품</th><th>출판사</th></tr></thead>
  <tbody id="rows"></tbody>
</table>
</div>

<footer>
  카카오페이지 웹소설 실시간 랭킹 · 리디 웹소설 오늘의 베스트 기준<br>
  순위 변동은 직전 수집(2026-11-05) 대비 · Claude 자동 수집 데이터
</footer>
</div>

<script>
const DATA = {
 asOf: "2026-11-09",
 prevDate: "2026-11-05",
 kakao: {label:"카카오페이지 웹소설 실시간 랭킹", items:[
  [1,"나는 이 빙의를 끝내려 한다","신솔라","연담"],
  [2,"전쟁영웅의 비밀을 나만 알아","망고킴","진주북스"],
  [3,"마법학교 마법사로 살아가는 법","글쓰는기계","블루픽스튜디오"],
  [4,"회귀한 재벌 손자는 용서가 어렵다","김광수","KT스튜디오지니1"],
  [5,"천재 궁수의 스트리밍","멍멍킴","kwbooks"],
  [6,"초월자들의 호감 뉴비가 되었다","갈색팬더","디앤씨미디어"],
  [7,"괴담에 떨어져도 출근을 해야 하는구나","백덕수","kwbooks"],
  [8,"남주가 죽은 아내를 찾다가 미쳐간다는데, 그 아내가 나다","린아(潾娥)","kwbooks"],
  [9,"신이 깃든 검술천재","흑아인","디앤씨미디어"],
  [10,"아카데미 절대 교수","임제열","kwbooks"],
  [11,"의원, 다시 살다","태선","고렘팩토리"],
  [12,"멸망한 세계의 4급 인간","외투","제이플미디어"],
  [13,"은해상단 막내아들","향란","디앤씨미디어"],
  [14,"이번 생은 우주대스타","손윤","디앤씨미디어"],
  [15,"초심 잃은 아이돌을 위한 회귀 백서","리벨","디앤씨미디어"],
  [16,"전직 K자영업자가 장사를 너무 잘함","유지민","삼양씨앤씨"],
  [17,"회사원이 퇴마를 잘함","호안","마야마루출판사"],
  [18,"천마등선","은열","JK미디어"],
  [19,"무당기협","은열","JK미디어"],
  [20,"어린 신인데 키워주시죠","강토공","넥스트 프로덕션"],
  [21,"고립된 후작 영애는 먹고살고 싶어서","유나진","연담"],
  [22,"무명 여배우인데 모두 날 알아","크트밀","연담"],
  [23,"1년차 만렙 매니저","현신","고렘팩토리"],
  [24,"우검쟁패","우각","스마트빅"],
  [25,"네크로맨서 학교의 소환천재","일제사격","kwbooks"],
  [26,"편집자의 생존수칙","김영지","디앤씨미디어"],
  [27,"회귀자 멱살 잡고 데뷔합니다","제르메","판시아"],
  [28,"은퇴하려고 결혼했는데, 상대가 황제다","정오찬","피플앤스토리"],
  [29,"천하제일 시골 무관","인재","JK미디어"],
  [30,"미친 빌런이 정신을 되찾음","김현우","글로번"]
 ], prev:{"남주가 죽은 아내를 찾다가 미쳐간다는데, 그 아내가 나다":1,"회귀한 재벌 손자는 용서가 어렵다":2,"은퇴하려고 결혼했는데, 상대가 황제다":3,"천재 궁수의 스트리밍":4,"탑에 살림차렸더니, 돈이 너무 쉬움":5,"괴담에 떨어져도 출근을 해야 하는구나":6,"신이 깃든 검술천재":7,"잊혀진 들판":8,"마법학교 마법사로 살아가는 법":9,"전쟁영웅의 비밀을 나만 알아":10,"멸망한 세계의 4급 인간":11,"의원, 다시 살다":12,"회사원이 퇴마를 잘함":13,"미친 빌런이 정신을 되찾음":14,"초심 잃은 아이돌을 위한 회귀 백서":15,"은해상단 막내아들":16,"이번 생은 우주대스타":17,"어린 신인데 키워주시죠":18,"고립된 후작 영애는 먹고살고 싶어서":19,"무명 여배우인데 모두 날 알아":20,"하트 어택(Heart attack)":21,"템빨":22,"아카데미 절대 교수":23,"무당기협":24,"데뷔 못 하면 죽는 병 걸림":25,"나는 이 빙의를 끝내려 한다":26,"우검쟁패":27,"마법명가 차남으로 살아남는 법":28,"1년차 만렙 매니저":29,"돈지랄 네크로맨서":30}},
 ridi: {label:"리디 웹소설 오늘의 베스트", items:[
  [1,"첫 실패","박영","나인"],
  [2,"미친놈을 꼬시고 튄 거 같다","무잔약","비욘드"],
  [3,"곧 죽을 악역수가 연쇄 고백마면 곤란한가요?","서리박쥐","텐시안"],
  [4,"북부대공의 애착부적이 되었다","AH무개","에페Epee"],
  [5,"총총","톨쥬","비욘드"],
  [6,"반석부터 시작하는 영지 생활 백서","버터앙팡","에페Epee"],
  [7,"말랑말랑 까칠한 S급 길들이기","감자콩도리","시크노블"],
  [8,"마굴","미나토","블랙엔"],
  [9,"연프 속 빌런이 하트를 받는 방법","해니니","킨블"],
  [10,"여우 후궁이 꼬리를 숨김","탈랑탈랑","텐시안"],
  [11,"흑호 가문 허스키입니다만","우울또","페브"],
  [12,"그 공포 게임의 진엔딩을 보는 법","테네시","모드"],
  [13,"후궁에서 존버하다 황제한테 집착당함","산달목","모드"],
  [14,"와일드 체리 버스터(Wild Cherry Buster)","민혜윤","신영미디어"],
  [15,"무구한 황제의 불손한 소년","임유니","비욘드"],
  [16,"용호상박 로맨스","표고밥","모드"],
  [17,"하나의 더치","건어물녀","에이블"],
  [18,"직무 유기가 이렇게 무섭습니다","샴록","BLYNUE 블리뉴"],
  [19,"터닝(Turning)","쿠유","비올렛"],
  [20,"내게 금지된 것","홍세라","블랙엔"],
  [21,"어떤 사랑","장소영","나인"],
  [22,"병약 무해한 황자로 살아남는 법","시종장","에페Epee"],
  [23,"마교 교주를 애지중지 키웠다","단애","블릿"],
  [24,"천운","채팔이","넥스트 프로덕션"],
  [25,"유괴는 상냥하게","Dips","텐시안"],
  [26,"재혼은 셋이서 합니다!","짜단","수아레"],
  [27,"비서의 퇴사가 서브 광공에게 미치는 영향","임이주","킨블"],
  [28,"올리브는 겨울에도 푸르다","사계귤","오브미디어"],
  [29,"대공님의 저주가 맛있습니다","먐개","모드"],
  [30,"빙의자의 팔은 안으로 굽는다","진도넛","BLYNUE 블리뉴"]
 ], prev:{"미친놈을 꼬시고 튄 거 같다":1,"천운":2,"하나의 더치":3,"첫 실패":4,"악역의 세계 구원법":5,"병약 무해한 황자로 살아남는 법":6,"키스 앤 퍽(Kiss and Puck)":7,"길줍한 족제비가 탐정이다":8,"북부대공의 애착부적이 되었다":9,"조연을 위한 회귀 지침서":10,"은행원도 용꿈을 꾸나요":11,"아가씨는 그 개가 갖고 싶다":12,"내게 금지된 것":13,"저 나쁜 사람 아닙니다 불쌍한 사람입니다":14,"사이프러스 모자이크":15,"디플라워 미 이프 유 캔(Deflower Me If You Can)":16,"반석부터 시작하는 영지 생활 백서":17,"페레니케":18,"연프 속 빌런이 하트를 받는 방법":19,"여우 후궁이 꼬리를 숨김":20,"마굴":21,"터닝(Turning)":22,"여기 고양이 카페 아니다":23,"색순응":24,"그 공포 게임의 진엔딩을 보는 법":25,"우는 아이에게는 대디 베어를":26,"용이 없는 용기사를 위한 반려용 길들이기 지침서":27,"NPC 활용 영역":28,"헌터물 흑막에 빙의했는데 적성에 너무 잘 맞음":29,"용호상박 로맨스":30}}
};

let cur = 'kakao';
const rows = document.getElementById('rows');
const q = document.getElementById('q');
document.getElementById('dateLine').textContent = DATA.asOf + ' 수집 기준 · Top 30';

function medal(r){ return r===1?'🥇':r===2?'🥈':r===3?'🥉':r; }
function chgHtml(title, rank, prev){
  if (!(title in prev)) return '<span class="new">NEW</span>';
  const d = prev[title] - rank;
  if (d > 0) return '<span class="up">▲ ' + d + '</span>';
  if (d < 0) return '<span class="down">▼ ' + (-d) + '</span>';
  return '–';
}
function render(){
  const p = DATA[cur];
  const kw = q.value.trim().toLowerCase();
  rows.innerHTML = p.items
    .filter(([r,t,a,pub]) => !kw || (t+a+pub).toLowerCase().includes(kw))
    .map(([r,t,a,pub]) =>
      '<tr><td class="rank' + (r<=3?' top3':'') + '">' + medal(r) + '</td>' +
      '<td class="chg">' + chgHtml(t, r, p.prev) + '</td>' +
      '<td><div class="title">' + t + '</div><div class="meta">' + a + '</div></td>' +
      '<td class="pub">' + pub + '</td></tr>'
    ).join('') || '<tr><td colspan="4" style="text-align:center;color:var(--sub);padding:30px">검색 결과 없음</td></tr>';
}
document.querySelectorAll('.tab').forEach(b => b.addEventListener('click', () => {
  cur = b.dataset.p;
  document.querySelectorAll('.tab').forEach(x => x.classList.toggle('active', x===b));
  render();
}));
q.addEventListener('input', render);
render();
</script>
</body>
</html>
