# Deputy Resources

Resources allow for easy creation of navigation based on users permissions.

## Anatomy of an Resource

Given the power of routes in Kohana, resources were designed to effectively map to an URI. There 
are conventions that make resources flexible and easy.

## Creating Resources

ACL_Resource uses a unified construct with the following parameters:

	$config = array
	(
		'uri'		=> 'forum',
		'title'		=> 'Forum',
		'visible'	=> TRUE,
		'override'	=> NULL
	);

The only required parameter is the URI. The title is derived from the last segment in the URI. 
Visibility is by default TRUE. Override can be used to specify another URI for the resource.

	$resource = ACL_Resource::factory(array('uri' => 'forum/thread'));
	
	// Outputs "Thread"
	echo $resource->get_title();
	
	// Outputs "forum/thread"
	echo $resource->get_uri();
	
	// Outputs "TRUE"
	var_export($resource->is_visible());

## Children Resources

### Setting

Children resources can be manually added to a parent.

	$parent = ACL_Resource::factory(array('uri' => 'forum'));
	
	$child = ACL_Resource::factory(array('uri' => 'forum/thread'));
	
	$acl_resource->set('thread', $child);

Of course, using the ACL takes care of the creation of children automatically.

### Getting

Retrieving children are easy. Simply "get" using the segment name:

	$child = $parent->get('thread');

### Traversing

ACL_Resource extends ArrayIterator allowing for easy traversal of children. A resource can 
essentially be used as an array.

	// Outputs "Thread"
	foreach ($parent as $child)
	{
		echo $child->get_title();
	}

## ACL Conventions

Deputy has flexible configuration for defining resources.

	$acl = ACL::instance();
	
	$acl->set_resources(array
	(
		'forum',
		'forum/thread' 			=> 'Threads',
		'forum/thread/add'		=> array('title' => 'Add Thread', 'visible' => TRUE),
		'forum/thread/edit'		=> FALSE,
		'forum/thread/delete'	=> TRUE,
		'forum/post'			=> ACL_Resource::factory(array('uri' => 'forum/post'))
	));