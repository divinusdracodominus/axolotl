# Compile Time Functions

compile time functions are first class, in that they can be invoked literally as normal functions with the exception of the # mark

example signature: 
```

interface Token: pub {
    func name(self: &impl Token) -> str;
    func id(self: &impl Token) -> u64;
    func kind(self: &impl Token) -> TokenType;
}

struct Field: pub {
    name: str,
    // name of the type, that can be looked up in the token stream
    type: str,
}

struct StructToken: pub {
    struct_name: str,
    id: u64,
    fields: Array<Field>
}

impl Token for StructToken;
func name(self: &StructToken) -> str {
    let myarray: Array<str, GC> = Array::new();
    $self.name.clone()
}

func id(self: &StructToken) -> u64 {
    $self.id
}

func kind(self: &StructToken) -> TokenType {
    TokenType::Struct
}

/// token stream is yet to be determined
enum CompResult<T: Token>: pub {
    Ok(TokenStream<T>),
    Err(str),
}

func my_compile_time_function<T>(types: map<str, TokenStream>, stream: TokenStream<T>) -> CompResult<T> {
    CompResult::Ok(stream)
}

// then to run the function
#my_compile_time_function(MyStruct)
struct MyStruct {
    name: String,
    value: u32,
}
```

in the example above 