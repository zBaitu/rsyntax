rSyntax ---- Rust libsyntax for rFmt
===============================

https://github.com/zBaitu/rsyntax

Overview
----------
rSyntax is cloned from [Rust libsyntax](https://github.com/rust-lang/rust/tree/master/src/libsyntax). It's only used for [rFmt](https://github.com/zBaitu/rfmt), another Rust source code formatter.

Features
----------
Only 3 file are changed.
* [lib.rs](https://github.com/zBaitu/rsyntax/tree/master/src/lib.rs)  
Add some pub use.
* [ast.rs](https://github.com/zBaitu/rsyntax/tree/master/src/ast.rs)  
Add Debug for some struct/enum.
* [parser.rs](https://github.com/zBaitu/rsyntax/tree/master/src/parse/parser.rs)  
Skip sub mod parse, and make the high position equals to the low position of the sub mod span, to specify this is a sub mod.

```
    /// Parse a `mod <foo> { ... }` or `mod <foo>;` item
    fn parse_item_mod(&mut self, outer_attrs: &[Attribute]) -> PResult<'a, ItemInfo> {
        let id_span = self.span;             
        let id = self.parse_ident()?;        
        if self.check(&token::Semi) {        
            self.bump();                     
            // baitu
            // This mod is in an external file. Let's go get it!
            // let (m, attrs) = self.eval_src_mod(id, outer_attrs, id_span)?;
            // Ok((id, m, Some(attrs)))       
            Ok((id, ItemKind::Mod(           
                Mod {                        
                    inner: mk_sp(id_span.lo, id_span.lo),              
                    items: Vec::new(),       
                }),
                None))
```
