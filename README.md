PHP-MySQL-Class
===============

Sorry for my bad english ( _ _)

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

Fetch array with binding data (auto escape)
<pre>
$data = array(
	':sex'	=> 'M',
);

/*
 * $mysql->fetchAll($query, $data = null, $result_type = MYSQL_BOTH)
 * 
 * @param $result_type
 *	MYSQL_ASSOC, MYSQL_NUM or MYSQL_BOTH
 *
 * @return result as array or FALSE on failure.
 */
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

Execute query with binding data (auto escape)
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

Insert row
<pre>
// insert('table name', $data_array)
$success = $mysql->insert('user', array(
	'name'	=> 'mika',
	'sex'	=> 'F',
));

if (! $success) {
	echo $mysql->error();
}
