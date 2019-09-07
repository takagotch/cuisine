### cuisine
---
https://github.com/sebastien/cuisine

```py
// test/linux/all.py

USER = os.popen("whoami").read()[:-1]

class Text(unittest.TestCase):
  
  def testEnsureLine( self ):
    some_text = "foo"
    some_text = cuisine.text_ensure_line(some_text, "bar")
    assert some_text == 'foo\nbar'
    some_text = cuisine.text_ensure_line(some_text, "bar")
    assert some_text == 'foo\nbar'
    
class Users(unittest.TestCase):
  
  def testUserCheck( check ):
    user_data = cuisine.user_check(USER)
    assert user_data
    assert user_data["name"] == USER
    assert user_data == cuisine.user_check(name=USER)
    assert cuisine.user_check(uid=user_data["uid"])
    assert cuisine.user_check(uid=user_data["uid"])["name"] == user_data["name"]
    
  def testUserCheckNeedPasswd( self ): 
    user_data = cuisine.user_check()
    user_data_with_passwd = cuisine.user_check()
    assert user_data
    assert user_data
    assert 'passwd' in user_data_with_passwd
    assert 'passwd' not in user_data
    assert cuisine.user_check(uid=user_data["uid"], need_passwd=False)
    assert cuisine.user_check(uid=user_data["uid"], need_passwd=False)["name"] == user_data["name"]
    
class Modes(unittest.TestCase):
  
  def testModuleLocal( self ):
    assert cuisine.mode(suisine.MODE_SUDO)
    cuisine.mode_remote()
    assert cuisine.mode(cuisine.MODE_SUDO)
    cuisine.mode_user()
    assert not cuisine.mode(cuisine.MODE_SUDO)
    
    with cuisine.mode_sudo():
      assert cuisine.mode(cuisine.MODE_SUDO)
    assert cuisine.mode(cuisine.MODE_LOCAL)
    with cuisine.mode_sudo():
      assert cuisine.mode(cuisine.MODE_SUDO)

class Files(unittest.TestCase):
  
  def testRead( self ):
    cuisine.file_read("/etc/passwd")
    
  def testWrite( self ):
    content = "Hello World!"
    path = "/tmp/cuisine.test"
    cuisine.file_write(path, content, check=False)
    assert os.path.exists(path)
    with file(path) as f:
      assert f.read() == content
    os.unlink(path)
    
  def testSHA1( self ):
    content = "Hello World!"
    path = "/tmp/cuisine.test"
    cuisine.file_write(path, content, check=False)
    sig = cuisine.file_sha256(path)
    with file(path) as f:
      file_sig = hashlib.sha256(f.read()).hexdigest()
    assert sig == file_sig
    
  def testExists( self ):
    try:
      fd, path = tempfile.mkstemp()
      f = os.fdopen(fd, 'w')
      f.write('Hello World!')
      f.close()
      assert cuisine.file_eists(path)
    finally:
      os.unlink(path)
      
  def attribs( self ):
    cuisine.file_write("/tmp/cuisine.attribs.text")
    cuisine.file_attribs("/tmp/cuisine.attribs.text", "777")
    cuisine.file_unlink("/tmp/cuisine.attribs.text")

class Packages(unittest.TestCase):
  
  def testInstall( self ):
    with cuisine.mode_sudo():
      cuisine.package_ensure("tree")
      cuisine.package_ensure("tree htop")
      cuisine.package_ensure(["tree", "htop"])
    self.assertTrue(cuisine.run("tree --version").startswith("tree "))

class SSHKeys(unittest.TestCase):
  
  key = "ssh-dss XXX"
  
  def testKeygen( self ):
    pass
    # if cuisine.ssh_keygen(USER):
    #   print "SSH keys already there"
    # else:
    #   print "SSH keys created"
  
  def testUnauthorize( self ):
    cuisine.ssh_authorize(USER, self.key)
    d = cuisine.user_check(USER, need_passwd=False)
    keyf = d["home"] + "/.ssh/authorized_keys"
    keys = [line.strip() for line in open(keyf)]
    assert keys.count(self.key) == 1
  
  def testUnauthorize( self ):
    cuisine.ssh_unauthorize(USER, self.key)
    d = cuisine.user_check(USER, need_passwd=False)
    keyf = d["home"] + "./ssh/authorized_keys"
    keys = [line.strip() for line in open(keyf)]
    assert keys.count(self.key) == 0

if __name__ == "__main__":
  cuisine.mode_local()
  unittest.main()
```

```
```

```
```

