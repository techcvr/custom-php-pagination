# custom-php-pagination
``` PHP
<?php include 'header.php'; ?>
<?php
$cat = $_GET['cat'];
$sub_cat = $_GET['cid'];
$sql_cat = "SELECT * from `category` where `id` = '$sub_cat'";
$res_cat = $mysqli->query($sql_cat) or die($mysqli->error);
$row_cat = $res_cat->fetch_assoc();

$limit = 15;
define ("MAX",$limit,true);

$sql4 = "SELECT * from `product_list` where `sub_cat` = '$sub_cat'";
if ($_GET[chid]) {
    $chid = $_GET[chid];
    $sql4 .= " and `child_cat` = '$chid'";
}
$sql4 .= " order by `id` desc";

$res4 = $mysqli->query($sql4) or die($mysqli->error);
$num1 = $res4->num_rows;

if (!$_GET[page])
{
    $start=0;
    $_GET[page]=1;
}
if($_GET[page])
{
    $start=(($_GET [page]-1)*MAX);
}
$num_page=ceil($num1/MAX);

$sql = "SELECT * from `product_list` where `sub_cat` = '$sub_cat'";
if ($_GET[chid]) {
    $chid = $_GET[chid];
    $sql .= " and `child_cat` = '$chid'";
}
$sql .= " order by `id` desc";
$sql .= " limit $start,".MAX;
// echo $sql;
// echo "<br>";
// echo $sql1;
// die;
$res = $mysqli->query($sql) or die($mysqli->error);
$num = $res->num_rows;

if($num>=1)
{
    while ($row = $res->fetch_assoc())
    {
      ?>
					<a href="product-details.php?id=<?php echo strip_tags($row['id']); ?>">
					    <div class="new_products_page_img_text_back">
                  <div class="new_products_page_img" style="background:url(<?php echo ltrim($row['thumbnail'],"../");?>) center top; background-size: cover;"><img src="                            <?php echo ltrim($row['thumbnail'],"../");?>"/></div>
                   <div class="new_products_page_text">
                      <?php 
                      echo substr(strip_tags($row['pname']), 0, 20);
                      if(strlen(strip_tags($row['pname'])) > 20)
                      {
                          echo '...';
                      }
                      ?>
                  </div>
               </div>
            </a>
		    <?php
			}
	}
?>
   
 
   
   
   
<div class="pagination_mane_back">
	<nav data-pagination="">
		<?php
		if ($_GET[page]>1)
		{
			$t=$_GET[page]-1;
			echo '<a href="product-category.php?cat='.$cat.'&cid='.$sub_cat.'&page='.$t.'"><i class="fa fa-arrow-left"></i></a>';
		}
		else {
			echo '<a href="#"><i class="fa fa-arrow-left"></i></a>';
		}
		?>

		<ul>
			<?php
			$start = 1;
			$slice = 3;

			if(!isset($_GET['page'])){
			$page = 1;
			} else {
			$page = $_GET['page'];
			}

			if (($page + $slice) < $num_page) {
			$this_far = $page + $slice;
			} else {
			$this_far = $num_page;
			}

			if (($start + $page) >= 3 && ($page - 3) > 0) {
				$start = $page - 3;
			}

			for ($i = $start; $i <= $this_far; $i++){
				if($i == $page){
					echo '<li class=current><a href="#">'.$i.'</a></li>';
				}
				else{
					echo '<li><a href="product-category.php?cat='.$cat.'&cid='.$sub_cat.'&page='.$i.'">'.$i.'</a></li>';
				}
			}
			?>
		</ul>
		<?php
		if ($_GET[page]<$num_page)
		{
			$v=$_GET[page]+1;
			echo '<a href="product-category.php?cat='.$cat.'&cid='.$sub_cat.'&page='.$v.'"><i class="fa fa-arrow-right"></i></a>';
		}
		else {
			echo '<a href="#"><i class="fa fa-arrow-right"></i></a>';
		}
		?>
	</nav>
</div>

<?php
if ($_GET[page]>1)
{
    $t=$_GET[page]-1;
    echo '<a href="product-category.php?cat='.$cat.'&cid='.$sub_cat.'&page='.$t.'"><div class="pagination_arrow"><i class="fa fa-arrow-left"></i></div></a>';

}
else {
    echo'<a href="javascript:(void)"><div class="pagination_arrow"><i class="fa fa-arrow-left"></i></div></a>';
}
//==================================================
$start = 1;
$slice = 2;

if(!isset($_GET['page'])){
    $page = 1;
} else {
    $page = $_GET['page'];
}

if (($page + $slice) < $num_page) {
    $this_far = $page + $slice;
} else {
    $this_far = $num_page;
}

if (($start + $page) >= 2 && ($page - 2) > 0) {
    $start = $page - 2;
}

for ($i = $start; $i <= $this_far; $i++){
    if($i == $page){
        echo '<a href="#"><div class="pagination_number" style="background: #0e5066; color: #fff;">'.$i.'</div></a>';
    }
    else{
        echo '<a href="product-category.php?cat='.$cat.'&cid='.$sub_cat.'&page='.$i.'"><div class="pagination_number">'.$i.'</div></a>';
    }
}
//==================================================
if ($_GET[page]<$num_page)
{
    $v=$_GET[page]+1;
    echo '<a href="product-category.php?cat='.$cat.'&cid='.$sub_cat.'&page='.$v.'"><div class="pagination_arrow"><i class="fa fa-arrow-right"></i></div></a>';
}
else {
    echo '<a href="#"><div class="pagination_arrow"><i class="fa fa-arrow-right"></i></div></a>';;
}
?>
```
