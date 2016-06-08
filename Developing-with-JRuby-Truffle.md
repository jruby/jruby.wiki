## Getting started

See truffle/[README](https://github.com/jruby/jruby/tree/master/truffle).

## Code patterns

### Where to allocate helper nodes (a node used in another node)

* If the node does not use the DSL, you should allocate the helper node [lazily](#the-lazy-pattern).
* If the helper node is used by every specialization: either allocate the helper node eagerly as a `@Child` or use the lazy pattern.
```java
public abstract class MyNode extends RubyNode {
    @Child MetaClassNode metaClassNode;

    public MyNode(RubyContext context, SourceSection sourceSection) {
        super(context, sourceSection);
        metaClassNode = MetaClassNodeGen.create(context, sourceSection, null);
    }
...
```
* If the helper node is used by only one specialization: use `@Cached`.
```java
        @Specialization
        public long objectID(DynamicObject object,
                @Cached("createReadObjectIDNode()") ReadObjectFieldNode readObjectIDNode) {
            final Object id = readObjectIDNode.executeRead(object);
            ...
        }

        protected ReadObjectFieldNode createReadObjectIDNode() {
            return new ReadObjectFieldNode(Layouts.OBJECT_ID_IDENTIFIER);
        }
```
However, if the node already uses `@Cached` *and there are guards on the @Cached values*,
consider whether you want one helper node per Specialization instantiation or only one for the whole node.

#### The lazy pattern

* Otherwise use the lazy pattern which __*includes*__ the `execute*` call on the helper node.
```java
        @Child ToStrNode toStrNode;
        ...

        protected DynamicObject toStr(VirtualFrame frame, Object object) {
            if (toStrNode == null) {
                CompilerDirectives.transferToInterpreter();
                toStrNode = insert(ToStrNodeGen.create(getContext(), getSourceSection(), null));
            }

            return toStrNode.executeToStr(frame, object);
        }
```

If you want to call different methods on a helper node, then use a `getToStrNode()` helper which returns the helper node:
```java
        @Child ToStrNode toStrNode;
        ...

        protected ToStrNode getToStrNode() {
            if (toStrNode == null) {
                CompilerDirectives.transferToInterpreterAndInvalidate();
                toStrNode = insert(ToStrNodeGen.create(getContext(), getSourceSection(), null));
            }

            return toStrNode;
        }
```

## Polymorphic inline caches

When you use `@Cached` to create a Polymorphic Inline Cache you should add a `limit` property to set the maximum size of the cache, and add a corresponding entry to `Options`. For examples see https://github.com/jruby/jruby/commit/cbacdd5a2be32d74ed152a1c306beaa927e80e4e.