This solution required creates `encoding_rs` and `encoding_rs_io`
```rust
use encoding_rs_io::{DecodeReaderBytes, DecodeReaderBytesBuilder};

let file = File::open("/mnt/c/Users/Milad/Desktop/ifbrlc-20230805.txt").unwrap();  
let mut reader = BufReader::new(  
	DecodeReaderBytesBuilder::new()  
	.encoding(Some(encoding_rs::WINDOWS_1256))  
	.build(file)  
);  
  
let mut line = String::new();  
loop {  
	let bytes_read = reader.read_line(&mut line).unwrap();  
	if bytes_read == 0 {  
		break;  
	}  
	let isin = &line[11 .. 23];  
	let code = &line[42..46];  
	if code == "00A3" && isin == "IRO3AHIZ0001" {  
		println!("{}", line);  
	}  
	line.clear();  
}
```