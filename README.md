<?php require "database.php";?>
<?php 
require_once "vendor/autoload.php";
use Michelf\Markdown;

// Mengambil daftar file markdown dalam direktori /docs
$files = glob("files/*.md");

$results = [];
foreach ($files as $file) {
  $markdown = file_get_contents($file);

  similar_text($markdown, $_GET['q'], $percent);

  $results[$file] = $percent;
}

// Mengurutkan hasil pencarian berdasarkan presentase kemiripan
arsort($results);

?>
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<meta name="viewport" content="width=device-width, initial-scale=1">
	<title> Google Algeo</title>
	<link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
	<div id='wrapper'>
		<div id='form'>
			<div id='logo'>
				Search Engine
			</div>
			<div id='form-input'>
				<form method='get' actibon='search.php'>
					<input type='text' id='q' name='q' value="<?= $_GET['q']; ?>" placeholder='Masukkn Kata Kunci...' style="width:100%">
				</form>
			</div>

		<div id='result'>
				<?php foreach ($results as $file => $percent) : ?>
				<a class="item-search">
		 			<h2><?= basename(substr($file, 6), ".md")?></h2>
		 			<?php $markdown = file_get_contents($file); ?>
		 			<p>Jumlah Kata: <?= str_word_count($markdown) ?></p>
		 			<p>Tingkat Kemiripan: <?= $percent ?>%</p>
		 			<p><?= substr($markdown, 2, 10); ?></p>
	 			</a>
				<?php endforeach; ?>
	</div>
</div>
</div>
<div><center>Copyright &copy; Kelompok 6</div></center>

</body>
</html>
