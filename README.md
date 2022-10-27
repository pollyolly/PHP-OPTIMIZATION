## PHP-OPTIMIZATION

### PHP-ARRAY AND MEMORY OPTIMIZATION
```
Note: Storing Arrays in a variable have default max limit of 2gb of strings. 
      Increase the PHP memory limit if necessary.
```
#### SplFixedArray (FIXED ARRAY SIZE)
```
/* COMMON USE */
$initialMemory = memory_get_usage();
$units = 100000;
$array = []; //USING REGULAR ARRAY
for($i = 0; $i <$units; $i++){
   $array[$i] = $i;
}
$finalMemory = memory_get_usage();
$memoryUsage = ceil(($finalMemory - $initialMemory)/ (1024*1024));
// Memory Usage in MB
echo $memoryUsage;
```
```
/* BETTER USE */
//Using Fixed Array

$initialMemory = memory_get_usage();
$units = 100000;

//Must have the exact size of Array 
//$array can only have integers indexes $array[1] not $array['test']
$array = new SplFixedArray($units); //USING SplFixedArray

for($i = 0; $i <$units; $i++){
   $array[$i] = $i;
}
$finalMemory = memory_get_usage();
$memoryUsage = ceil(($finalMemory - $initialMemory)/ (1024*1024));
// Memory Usage in MB
echo $memoryUsage;
```
#### ARRAY REPETITIVE FUNCTION CALL
```
/* WRONG */
$array[trim($value)] = trim($value);

/* CORRECT */
$normalizedValue = trim($value);
$array[$normalizedValue] = $normalizedValue;

/* WRONG */
$array[$normalizer->normalize($value)] = $normalizer->normalize($value);

/* CORRECT */
$normalizedValue = $normalizer->normalize($value);
$array[$normalizedValue] = $normalizedValue;
```
#### SLOW ARRAY FUNCTION CALL
```
/* WRONG */
//This will call count() multiple times
for ($index = 0; $index < count($array); ++$index) {
   /* something happens here */
}
/* CORRECT */
//Initialized count() once in a variable
for ($index = 0, $count = count($array); $index < $count; ++$index) {
   /* something happens here */
}
$count = count($array);
for ($index = 0, $index < $count; ++$index) {
   /* something happens here */
}
```
#### STORING ARRAY IN JSON
```
/* STORING DATA */
$array[] = gzencode(json_encode($data));
/* USING DATA */
$myData = json_decode(gzdecode($array[0], true));
```
### PHP BUGS
```
/* WRONG */
//realpath() may cause bugs in other environment
define ('PROJECT_ROOT', realpath(__DIR__ . '/../'));
require_once realpath(__DIR__ . '/../../vendor/autoload.php');
    
/* CORRECT */
define ('PROJECT_ROOT', dirname(__DIR__) . '/');
require_once dirname(dirname(__DIR__)) . '/vendor/autoload.php';
```
### PHP ARCHITECTURE
```
//Class implements interfaces multiple times

/* WRONG */
interface Constract {}
class ParentClass implements Contract {}
class ChildClass extends ParentClass implements Contract {}

/* CORRECT */
interface Constract {}
class ParentClass implements Contract {}
class ChildClass extends ParentClass {}
```
### PHP SIMPLIFIED
#### IF RETURN 
```
/* BEFORE */
if ($variable === 'value') {
   return true;
}
return false;
    
/* AFTER */
return $variable === 'value';
```
#### SWITCH CASE 
```
/* BEFORE */
switch ($variable) {
   case 'value':
   /* operations: 1st batch */
   break;
   default:
   /* operations: 2nd batch */
   break;
}
/* AFTER */
if ($variable === 'value') {
   /* operations: 1st batch */
} else {
   /* operations: 2nd batch */
}
```
#### TERNARY OPERATOR
```
/* BEFORE */
$variable = $number > 0 ? true : false;
$variable = $number & $flag ? true : false;
    
/* AFTER */
$variable = $number > 0;
$variable = (bool) ($number & $flag);
```
#### UNSET
```
/* BEFORE */
    unset($variable);
    unset($argument);
    
/* AFTER */
    unset($variable, $argument);
```
#### ONE TIME USE VARIABLE
```
/* BEFORE */
    $object = new Clazz();
    return $object->method();
    
/* AFTER */
    return (new Clazz())->method();
```
#### USAGE OF LIST AS VARIABLE
```
/* BEFORE */
    $array  = [ ... ];
    $first  = $array[0];
    $second = $array[1];
    
/* AFTER */
    list($first, $second) = [ ... ];
    
//SAMPLE
$info = array('coffee', 'brown', 'caffeine');

// Listing all the variables
list($drink, $color, $power) = $info;
echo "$drink is $color and $power makes it special.\n";

// Listing some of them
list($drink, , $power) = $info;
echo "$drink has $power.\n";
```
#### array_search() REPLACED BY in_array()
```
/* BEFORE */
    if (array_search($what, $where, true) === false) {
        /* some logic here */
    }
    
/* AFTER */
    if (! in_array($what, $where, true)) {
        /* some logic here */
    }
```
#### strtr() REPLACED BY str_replace()
```
/* BEFORE */
    $normalizedPath = strtr($string, '\\', '/');
    
/* AFTER */
    $normalizedPath = str_replace('\\', '/', $string);
```
#### substr() REPLACED BY strpos()
```
/* BEFORE */
    $containsString = substr($where, 0, strlen($what)) === $what;
    
/* AFTER */
    $containsString = strpos($where, $what) === 0;
```
#### Redundant ELSE
```
/* BEFORE */
    if ($value !== 'forbidden-value') {
        return $value;
    } else {
        return null;
    }
/* AFTER */
    if ($value !== 'forbidden-value') {
        return $value;
    }
    return null;
```
#### Unncessary string case manipulation
```
/* BEFORE */
    $matched = preg_match('/^prefix/i', strtolower($string));
    
/* AFTER */
    $matched = preg_match('/^prefix/i', $string);
```
#### compact()
```
/* BEFORE */
    $array = ['first' => $first, 'second' => $second];
    
/* AFTER */
    $array = compact('first', 'second');
```
#### isset() can me merge
```
/* BEFORE */
    $value = isset($first) && isset($second);
    
/* AFTER */
    $value = isset($first, $second);
```
#### strlen() misused
```
/* BEFORE */
    if (strlen($string))   {}
    if (! strlen($string)) {}
    
/* AFTER */
    if ($string !== '') {}
    if ($string === '') {}
```
#### ob_get_clean() can be used
```
/* BEFORE */
    $content = ob_get_contents();
    ob_end_clean();

/* AFTER */
    $content = ob_get_clean();
```
