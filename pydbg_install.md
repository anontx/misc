# pydbg安装记录

*windows*, *python2.7*

## 1. 下载paimei
    
    [openrce.org](http://www.openrce.org/downloads/details/208/PaiMei)


## 2. 修改ctypes以支持python27

    c:\Python27\Lib\ctypes\__init__.py

        from _ctypes import Structure as _ctypesStructure       # add line
        from struct import calcsize as _calcsize
        class Structure(_ctypesStructure): pass                 # add line


## 3. 下载pydasm.pyd

    [pydasm.pyd](https://www.lfd.uci.edu/~gohlke/pythonlibs/#pydbg)


## 4. 替换pydasm.pyd

    c:\Python27\Lib\site-packages\pydasm.pyd



### reference

    * [axcheron/pydasm](https://github.com/axcheron/pydasm)
    * [openrce](http://www.openrce.org)
