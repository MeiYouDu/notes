# Hash
多个入口文件会用同一个hash值，如果一个文件改变，会导致其他文件的hash值名称变化
# contentHash
不相关的文件hash不同，但是如果一个文件中引入了另一个模块，且另一个模块需要独立打包时，两个hash值会相同，且改变父模块会影响子模块文件的hash值。
# chunkHash
如果需要把一个模块分成单独的包，建议用这个placeholder，他会生成独有的hash值。
