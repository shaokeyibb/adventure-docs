=====
书本
=====

构造一个书本
^^^^^^^^^^^^^^^^^^^^

书本由以下内容组成:
  * 一个组件, 被用作表示书本的标题
  * 一个组件, 被用作表示书本的作者
  * 一个组件集合, 被用作表示书页

**示例:**

.. code:: java

    //创建并为目标听众打开一个有关猫咪的书本
    public void openMyBook(final @NonNull Audience target){
        Component bookTitle = Component.text("Encyclopedia of cats");
        Component bookAuthor = Component.text("kashike");
        Collection<Component> bookPages = Cats.getCatKnowledge();

        Book myBook = Book.book(bookTitle, bookAuthor, bookPages);
        target.openBook(myBook);
    }

有关书本的额外信息
^^^^^^^^^^^^^^^^^^^^^^^^^^

adventure 中的书本不一定与客户端中的一个可交互的书本物品相关联.
在当前版本中, 这种关联需要在 adventure 外被实现.

任何超出游戏对每页文本限制的组件都会被客户端截断, 对于组件的数量 (即页码) 也是如此.
进一步了解这些限制可阅读 `Minecraft Wiki <https://minecraft.gamepedia.com/Book_and_Quill#Writing>`_


