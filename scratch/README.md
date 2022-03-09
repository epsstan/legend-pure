# M1

The M1 layer is composed of models that are written by end users.

```
Class Firm
{
    name: String[1];
}
```

# M2

The M2 layer defines the concepts like ```Class```, ```Association``` used in the M1 layer.

Here ```Class``` is ````meta::protocols::pure::vX_X_X::metamodel::domain::Class```` and not the Pure Class.

https://github.com/finos/legend-pure/blob/master/legend-pure-code-compiled-core%2Fsrc%2Fmain%2Fresources%2Fcore%2Fpure%2Fprotocol%2FvX_X_X%2Fmodels%2Fmetamodel.pure

```
Class meta::protocols::pure::vX_X_X::metamodel::domain::Class extends meta::protocols::pure::vX_X_X::metamodel::PackageableElement, meta::protocols::pure::vX_X_X::metamodel::domain::AnnotatedElement
{
   superTypes : String[*];
   constraints : Constraint[*];
   properties : meta::protocols::pure::vX_X_X::metamodel::domain::Property[*];
   qualifiedProperties : meta::protocols::pure::vX_X_X::metamodel::domain::QualifiedProperty[*];
   originalMilestonedProperties : meta::protocols::pure::vX_X_X::metamodel::domain::Property[*];
}
```

The M2 layer also defines other DSLs like ```RelationalConnection```

https://github.com/finos/legend-pure/blob/master/legend-pure-code-compiled-core-relational%2Fsrc%2Fmain%2Fresources%2Fcore_relational%2Frelational%2Fprotocols%2Fpure%2FvX_X_X%2Fmodels%2Fmetamodel_connection.pure


```
Class meta::protocols::pure::v1_22_0::metamodel::store::relational::connection::RelationalDatabaseConnection extends meta::protocols::pure::v1_22_0::metamodel::store::relational::connection::DatabaseConnection
{
   datasourceSpecification:meta::protocols::pure::v1_22_0::metamodel::store::relational::connection::alloy::specification::DatasourceSpecification[1];
   authenticationStrategy:meta::protocols::pure::v1_22_0::metamodel::store::relational::connection::alloy::authentication::AuthenticationStrategy[1];
   postProcessors: meta::protocols::pure::v1_22_0::metamodel::store::relational::postProcessor::PostProcessor[*];
}
```

## Questions
* Why is RelationalDatabaseConnection not defined in legend-pure-m2-store-relational

# M3

The M3 layer defines the Pure ```Class```, ```Property``` etc.

The M3 components are described in terms of ```CoreInstance```

## Questions 
* What is this syntax in ```legend-pure-m3-core/m3.pure``` 
```
^Root.children[meta].children[pure].children[metamodel].children[type].children[Class] Class @Root.children[meta].children[pure].children[metamodel].children[type].children
{
...
}

```
# M4

M4 defines ```CoreInstance```


# Parsing 

```
See TestM3AntlrParser, AntlrContextToM3CoreInstance.classParser

  @Test
    public void testSimpleClass()
    {
        String code = "Class Person\n" +
                "      {\n" +
                "         firstName: String[1];" +
                "         lastName: String[1];" +
                "      }\n" +
                "Class datamarts::DataM23::domain::BatchIDMilestone\n" +
                "{   \n" +
                "   inZ: Date[1];\n" +
                "   outZ: Date[1];\n" +
                "   vlfBatchIdIn: Integer[1];\n" +
                "   vlfBatchIdOut: Integer[1];   \n" +
                "   vlfDigest : String[1];\n" +
                "   \n" +
                "   filterByBatchID(batchId: Integer[1]) { $this.vlfBatchIdIn == $batchId; } : Boolean[1];\n" +
                "   filterByBatchIDRange(batchId: Integer[1]) { ($batchId >= $this.vlfBatchIdIn) && ($batchId <= $this.vlfBatchIdOut) ; } : Boolean[1];\n" +
                "   // Pure does not support contains or in today. Hence, difficul to support multiple batch id\n" +
                "   //filterByBatchIDs(batchIds: Float[*]) { and ($batchIds->map(batchId|$batchId == $this.vlfBatchIdIn)); } : Boolean[1];\n" +
                "   filterByProcessingTime(processingTime: Date[1]) { ($processingTime >= $this.inZ) && ($processingTime <= $this.outZ); } : Boolean[1];\n" +
                "}";
        new M3AntlrParser(null).parse(code, "test", true, 0, this.repository, this.newInstances, this.stateListener, this.context, 0, null);
    }
```
There are a set of parsers. Each parser is (mostly) code generated from a grammar.
* The M3Parser.g4 grammar defines the grammar for the M1 class. The parser produces instances of CoreInstance
* See ParserLibrary 
* How does the RelationalDatabaseConnection get parsed ?

# Java Code Generation
* CoreInstanceGenerator ??