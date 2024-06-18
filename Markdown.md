## Markdownチートシートのリンク  
[Google]:  https://gist.github.com/mignonstyle/083c9e1651d7734f84c99b8cf49d57fa#file-markdown-cheatsheet-md  
[チートシートリンク][Google]   

  #別のタブでリンクを開く方法  
  以下の関数を追加するとできる
<script type="text/javascript" src="//ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js"></script>
<script language="JavaScript">
$(document).ready( function () {
   $("a[href^='http']:not([href*='" + location.hostname + "'])").attr('target', '_blank');
})
</script>
  
