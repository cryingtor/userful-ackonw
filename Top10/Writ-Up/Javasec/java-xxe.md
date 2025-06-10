# java-xxe
```
DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
// 关键防御配置[1,6](@ref)：
dbf.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true);
dbf.setFeature("http://xml.org/sax/features/external-general-entities", false);
dbf.setFeature("http://xml.org/sax/features/external-parameter-entities", false);
dbf.setExpandEntityReferences(false); // 禁止实体展开
```