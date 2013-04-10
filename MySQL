<?php
/*
 *  Copyright (C) 2013
 *  Wing Leong
 *
 *  This program is free software: you can redistribute it and/or modify
 *  it under the terms of the GNU General Public License as published by
 *  the Free Software Foundation, either version 3 of the License, or
 *  (at your option) any later version.
 *
 *  This program is distributed in the hope that it will be useful,
 *  but WITHOUT ANY WARRANTY; without even the implied warranty of
 *  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 *  GNU General Public License for more details.
 *
 *  You should have received a copy of the GNU General Public License
 *  along with this program.  If not, see <http://www.gnu.org/licenses/>.
*/

class MySQL {
  
	public $connection = false;
	
	/*
	 * Constructor
	 *
	 * @throw Exception on failure.
	 */
	
	function __construct($host, $username, $password, $database = null) {
		$this->connection = mysql_connect($host, $username, $password);
		
		if ($this->connection === false) {
			throw new Exception(mysql_error());
		}
		
		if (! is_null($database)) {
			// Select database
			if (! $this->use_db($database)) {
				// Select database failed
				throw new Exception(mysql_error());
			}
		}
	}
	
	/*
	 * @return Last MySQL error message
	 */
	function error() {
		return mysql_error($this->connection);
	}
	
	/*
	 * Use database
	 *
	 * @return TRUE on success or FALSE on failure.
	 */
	function use_db($database) {
		if (! $this->connection) {
			return false;
		}
		
		return mysql_select_db($database, $this->connection);
	}
	
	/*
	 * Execute query
	 *
	 * @param $data = [':key' => 'value', ... ];
	 *	Bind data to query, auto escape
	 *
	 * @return Resource or TRUE on success or FALSE on failure.
	 */
	function executeQuery($query, $data = null) {
		if (! $this->connection) {
			return false;
		}
		
		// Bind data to query
		if (! is_null($data)) {
			foreach ($data as $key => $value) {
				// Escape data
				$value = mysql_real_escape_string($value, $this->connection);
				
				if ($value === false) {
					// Escape string failure
					return false;
				}
				
				$query = str_replace($key, "'$value'", $query);
			}
		}
		
		return mysql_query($query, $this->connection);
	}
	
	/*
	 * Execute query, and fetch result as array
	 *
	 * @param $data = [':key' => 'value', ... ];
	 *	Bind data to query, auto escape
	 *
	 * @param $result_type
	 *	MYSQL_ASSOC, MYSQL_NUM or MYSQL_BOTH
	 *
	 * @return result as array or TRUE on success or FALSE on failure.
	 */
	function fetchAll($query, $data = null, $result_type = MYSQL_BOTH) {
		$resource = $this->executeQuery($query, $data);
		
		if ($resource === false) {
			// Query failed
			return false;
		} elseif ($resource === true) {
			// Query is not a 'select' statment, may be 'insert', 'delete'...
			return true;
		}
		
		// Fetch result
		$result = array();
		while ($row = mysql_fetch_array($resource, $result_type)) {
			$result[] = $row;
		}
		
		return $result;
	}
	
	/*
	 * Insert row into table
	 *
	 * @param $row = ['column_name' => 'value', ... ];
	 *
	 * @return Resource or TRUE on success or FALSE on failure.
	 */
	function insert($table, $row) {
		if (! $this->connection || ! is_array($row) || count($row) == 0) {
			return false;
		}
		
		$table = mysql_real_escape_string($table, $this->connection);
		
		$keys = array();
		$values = array();
		
		// Escape data
		foreach ($row as $key => $value) {
			$keys[] = "`" . mysql_real_escape_string($key, $this->connection) . "`";
			$values[] = "'" . mysql_real_escape_string($value, $this->connection) . "'";
		}
		
		// Convert keys and values to string
		$keys = join(',', $keys);
		$values = join(',', $values);
		
		return mysql_query("INSERT INTO `{$table}` ({$keys}) VALUES ({$values})");
	}
}
