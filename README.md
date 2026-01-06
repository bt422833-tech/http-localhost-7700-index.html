<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<title>Trắc nghiệm Hóa học</title>
<style>
body { font-family: Arial; padding: 20px; }
.question { margin-bottom: 18px; }
.correct { color: green; font-weight: bold; }
.wrong { color: red; font-weight: bold; }
button { padding: 10px 18px; margin: 10px 5px; font-size: 15px; }
.answer-detail { margin-top:5px; padding-left:15px; }
</style>
</head>
<body>

<h2>TRẮC NGHIỆM HÓA HỌC</h2>
<form id="quiz"></form>

<button onclick="submitQuiz()">Nộp bài</button>

<h3 id="result"></h3>

<script>
// Hàm xáo trộn mảng
function shuffle(arr){
  for(let i=arr.length-1;i>0;i--){
    let j=Math.floor(Math.random()*(i+1));
    [arr[i],arr[j]]=[arr[j],arr[i]];
  }
}

// Dữ liệu câu hỏi
let mcq = [
{q:"Câu 1. Polymer nào sau đây không phải là thành phần chính của chất dẻo?", a:["Poly(vinyl chloride)","Polyethylene","Poly(methyl methacrylate)","Polyacrylonitrile"], c:3},
{q:"Câu 2. Nguyên liệu chính để sản xuất HDPE là monomer nào?", a:["CH2=CH–C6H5","CH2=CH–Cl","CH2=CH–CH3","CH2=CH2"], c:3},
{q:"Câu 3. Hợp chất nào sau đây là este?", a:["CH3COOCH3","CH3COOH","CH3CHO","CH3COCH3"], c:0},
{q:"Câu 4. Amino acid X trong môi trường pH = 6 có thể là", a:["Lysine","Glycine","Alanine","Glutamic acid"], c:0},
{q:"Câu 5. Hợp chất carbohydrate trong hình có tên là", a:["Maltose","Fructose","Cellulose","Saccharose"], c:3},
{q:"Câu 6. Biện pháp nào sau đây không làm giảm rác thải nhựa?", a:["Tái chế đồ nhựa","Khuyến khích dùng túi nilon","Mang túi cá nhân","Vật liệu phân hủy sinh học"], c:1},
{q:"Câu 7. Polymer nào được điều chế bằng phản ứng trùng ngưng?", a:["Nylon-6,6","PE","PVC","PS"], c:0},
{q:"Câu 8. Ethyl acetate có công thức là", a:["CH3COOCH2CH2CH3","CH3COOCH=CH2","CH3COOC6H5","CH3COOC2H5"], c:3},
{q:"Câu 9. Đun sôi lòng trắng trứng xảy ra hiện tượng", a:["Đông tụ","Kết tủa đỏ gạch","Màu tím","Màu xanh lam"], c:0},
{q:"Câu 10. Triolein có công thức là", a:["(C17H33COO)3C3H5","(C17H31COO)3C3H5","(C17H35COO)3C3H5","(C15H31COO)3C3H5"], c:0},
{q:"Câu 11. Ethylamine có công thức là", a:["(CH3)2NH","C2H5NH2","C6H5NH2","CH3NH2"], c:1},
{q:"Câu 12. Phát biểu nào sau đây không đúng?", a:["Tơ tằm, nylon-6,6 đều là polymer thiên nhiên","Tinh bột là polymer thiên nhiên","PE là polymer tổng hợp","Nylon-6,6 là polymer trùng ngưng"], c:0},
{q:"Câu 13. Este có phân tử khối bằng 88 là", a:["HCOOC2H5","C5H11OH","C3H7COOH","CH3COOC2H5"], c:3},
{q:"Câu 14. Đơn vị cấu tạo nên polypeptide là", a:["Monosaccharide","α-amino acid","Glycerol","Acid béo"], c:1},
{q:"Câu 15. PVC được tổng hợp trực tiếp từ monomer", a:["Vinyl acetate","Acrylonitrile","Vinyl chloride","Propylene"], c:2},
{q:"Câu 16. Lưu hóa cao su thuộc loại phản ứng", a:["Tăng mạch polymer","Phân hủy polymer","Cắt mạch polymer","Giữ nguyên mạch polymer"], c:0},
{q:"Câu 17. Carbohydrate không phản ứng Tollens là", a:["Saccharose","Glucose","Fructose","Tinh bột"], c:0},
{q:"Câu 18. Glutamic acid có số nguyên tử O là", a:["4","3","2","1"], c:0},
{q:"Câu 19. Thành phần chính của xà phòng là", a:["CH3(CH2)16COOH","CH3(CH2)3COONa","CH3(CH2)11OSO3Na","CH3(CH2)14COONa"], c:3},
{q:"Câu 20. Tên thay thế của CH3NH2 là", a:["Methylamine","Ethanamine","Ethylamine","Methanamine"], c:3},
{q:"Câu 21. Glycine có số nhóm –COOH là", a:["2","1","3","4"], c:1},
{q:"Câu 22. Chất nào sau đây không làm đổi màu quỳ tím?", a:["Chất (3)","Chất (1)","Chất (2)","Chất (4)"], c:1},
{q:"Câu 23. Nhận xét nào sau đây không đúng?", a:["Fructose tham gia phản ứng tráng bạc","Glucose tan tốt trong nước","Glucose kém ngọt hơn saccharose","Cellulose + NaOH → glucose"], c:3},
{q:"Câu 24. Số nguyên tử carbon trong glucose là", a:["5","11","6","12"], c:2},
{q:"Câu 25. Chất nào sau đây là tripeptide?", a:["Ala–Val","Val–Gly","Gly–Ala–Val","Gly–Ala"], c:2},
{q:"Câu 26. Chất không tạo dung dịch xanh lam với Cu(OH)2 là", a:["Glycerol","Ethanol","Saccharose","Glucose"], c:1},
{q:"Câu 27. Chất thuộc loại carbohydrate là", a:["alanine","polyethylene","glycerol","cellulose"], c:3},
{q:"Câu 28. Thủy phân X, thu được ethyl alcohol. Tên gọi của X là", a:["methyl propionate","ethyl propionate","ethyl acetate","methyl acetate"], c:2},
{q:"Câu 29. Cho các phản ứng: HN-CH2-COOH + HCl → ?, HN-CH2-COOH + NaOH → ?", a:["Chỉ có tính acid","Có tính oxi hóa và tính khử","Chỉ có tính base","Có tính chất lưỡng tính"], c:3},
{q:"Câu 30. Polymer thiên nhiên X được sinh ra trong quá trình quang hợp của cây xanh. Ở nhiệt độ thường. X là", a:["tinh bột","cellulose","saccharose","glycogen"], c:0}
];

// Xáo trộn câu hỏi
shuffle(mcq);

let quiz = document.getElementById("quiz");

// Tạo câu hỏi + xáo trộn đáp án cho mỗi câu
mcq.forEach((x,i)=>{
  // Tạo bản sao của đáp án để xáo trộn
  let answers = x.a.map((text,index)=>({text, index}));
  shuffle(answers);
  x.shuffledAnswers = answers; // lưu vị trí sau xáo trộn

  quiz.innerHTML += `<div class="question" id="q${i}">
    <b>${x.q}</b><br>` +
    answers.map((o,j)=>`<label>
      <input type="radio" name="q${i}" value="${o.index}"> ${o.text}
    </label><br>`).join("") +
  `</div>`;
});

function submitQuiz(){
  let score = 0;
  mcq.forEach((x,i)=>{
    let r = document.querySelector(`input[name="q${i}"]:checked`);
    let div = document.getElementById("q"+i);
    let user = r ? parseInt(r.value) : null;
    if(user === x.c) score++;

    div.innerHTML += `<div class="answer-detail">
      Bạn chọn: <span class="${user===x.c?'correct':'wrong'}">${user!==null ? x.a[user] : "Chưa chọn"}${user===x.c?' ✅':' ❌'}</span> - 
      Đáp án đúng: <span class="correct">${x.a[x.c]}</span>
    </div>`;
  });
  document.getElementById("result").innerText = "Điểm: " + score + "/30";
}
</script>
</body>
</html>
 
