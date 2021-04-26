# los.rubiya.kr


<h2>1.gremlin</h2>
 <pre><code>
  include "./config.php";
  login_chk();
  $db = dbconnect();
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[id])) exit("No Hack ~_~"); // do not try to attack another table, database!
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[pw])) exit("No Hack ~_~");
  $query = "select id from prob_gremlin where id='{$_GET[id]}' and pw='{$_GET[pw]}'";
  echo "query :{$query}";
  $result = @mysqli_fetch_array(mysqli_query($db,$query));
  if($result['id']) solve("gremlin");
  highlight_file(__FILE__);
 </pre></code>
解:查看代碼並沒有特殊過濾or和and所以用普通的'or 1=1 #或者admin ' #都可以或者'or ture -- 或 admin ' -- 輸入後會跳出GREMLIN Clear!就代表可以到下一關。
  
<h2>2.cobolt</h2>
<pre><code>
  
  include "./config.php"; 
  login_chk();
  $db = dbconnect();
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[id])) exit("No Hack ~_~"); 
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[pw])) exit("No Hack ~_~"); 
  $query = "select id from prob_cobolt where id='{$_GET[id]}' and pw=md5('{$_GET[pw]}')"; 
  echo "query:{$query}"; 
  $result = @mysqli_fetch_array(mysqli_query($db,$query)); 
  if($result['id'] == 'admin') solve("cobolt");
  elseif($result['id']) echo "Hello {$result['id']}You are not admin :("; 
  highlight_file(__FILE__); 
  
</pre></code>
 解:因為pw有md5過濾所以注入在id後'or 1=1 %23他會說you are not admin，那我們嘗試後面再加一個limit 1,1 %23，admin' # cobolt Clear!。
 
<h2>3.goblin</h2>
<pre><code>
  
  include "./config.php"; 
  login_chk(); 
  $db = dbconnect(); 
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[no])) exit("No Hack ~_~"); 
  if(preg_match('/\'|\"|\`/i', $_GET[no])) exit("No Quotes ~_~"); 
  $query = "select id from prob_goblin where id='guest' and no={$_GET[no]}"; 
  echo "query : {$query}"; 
  $result = @mysqli_fetch_array(mysqli_query($db,$query)); 
  if($result['id']) echo "Hello {$result[id]}"; 
  if($result['id'] == 'admin') solve("goblin");
  highlight_file(__FILE__); 
  
</pre></code>
解:看到注入點no那先測試看看no=1，顯示Hello guest並且過濾了'和"那就先讓no=0，再加上or no=1看看是不是Hello guest，確定是那試試看把no=2，goblin Clear!。
