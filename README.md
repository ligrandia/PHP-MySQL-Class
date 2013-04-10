PHP-MySQL-Class
===============

A MySQL class base on PHP mysql,<br />
Base on mysqli, <a href="https://github.com/ajillion/PHP-MySQLi-Database-Class">MySQLi-Database-Class</a><br />
Base on PDO, <a href="http://www.doctrine-project.org/projects/dbal.html">DBAL</a>

Initialize

<pre>
require_once('MySQL.php');

try {
	$mysql = new MySQL('host', 'username', 'password', 'database');
} catch (Exception $e) {
	error($e->getMessage());
}
</pre>
or
<pre>
require_once('MySQL.php');

try {
	$mysql = new MySQL('host', 'username', 'password');
	$mysql->use_db($database);
} catch (Exception $e) {
	error($e->getMessage());
}
</pre>

<h2>Fetch array with binding data (auto escape)</h2>
<pre>
$data = array(
	':sex'	=> 'M',
);

// Return result as array or FALSE on failure.
$result = $mysql->executeQuery('SELECT * FROM user WHERE sex = :sex', $data);

if ($result === false) {
	// Failed
	echo $mysql->error();
}

var_dump($result);
</pre>

Output:

<pre>
array(2) {
  [0]=>
  array(2) {
    ["name"]=>
    string(4) "wing"
    ["sex"]=>
    string(1) "M"
  }
  [1]=>
  array(2) {
    ["name"]=>
    string(4) "john"
    ["sex"]=>
    string(1) "M"
  }
}
</pre>

<h2>Execute query with binding data (auto escape)</h2>
<pre>
$data = array(
	':name'	=> 'wing',
	':sex'	=> 'M',
);

// return Resource (select) or TRUE (insert, delete, ...) on success or FALSE on failure.
$result = $mysql->executeQuery('
	DELETE * FROM user
	WHERE name = :name AND sex = :sex',
	$data
);

if (! $result) {
	// Failed
	echo $mysql->error();
}
</pre>

<h2>Insert row</h2>
<pre>
// insert('table name', $data_array)
$success = $mysql->insert('user', array(
	'name'	=> 'mika',
	'sex'	=> 'F',
));

if (! $success) {
	echo $mysql->error();
}
</pre>

Sorry for my bad english, and I don't know how to use git ( _ _)
