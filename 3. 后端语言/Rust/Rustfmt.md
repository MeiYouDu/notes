rustfmt是一个格式化rust代码的仓库。
# 自动格式化代码
## 跟ide集成
ide可以集成rustfmt，在保存的代码的时候运行rustfmt，这样也就实现了自动格式化代码。
## 编译时运行
通过`cargo run --bin runstfmt -- filename`可以实现编译时前先格式化代码。（这个方法没使用的时候没有成功，提示名称不是bin，但是单独用个bin是可以使用的）。
