## General .NET Contributing Guide

1. Unless it is a trivial change, make sure that there is a corresponding issue for your change first. If there
   is none, create one.
2. Create a fork in GitHub
3. Create a branch off the `master` branch. Name it something that that makes
   sense, such as `issue-123` or `githubhandle-issue`. This makes it easy for everyone to figure out what
   the branch is used for. It also makes it easier to isolate your change from incoming changes from the origin.
4. Commit your changes and push your changes to GitHub
5. Create a pull request against the origin's `master` branch

## DOs and DON'Ts

* **DO** follow our coding style (see below)
* **DO** include tests when adding new features. When fixing bugs, start with
  adding a test that highlights how the current behavior is broken.
* **DO** keep the discussions focused. When a new or related topic comes up
  it's often better to create new issue than to side track the discussion.
* **DO** blog and tweet (or whatever) about your contributions, frequently!
* **DON'T** surprise us with big pull requests. Instead, file an issue and start
  a discussion so we can agree on a direction before you invest a large amount
  of time.
* **DON'T** commit code that you didn't write. If you find MIT or Apache 2 licensed code that you think is a good fit to add to .NET Core, file an issue and start a discussion before proceeding.
* **DON'T** add API additions without filing an issue and discussing with us first.

## C# Coding Style

The general rule we follow is "use Visual Studio defaults".

1. We use [Allman style](http://en.wikipedia.org/wiki/Indent_style#Allman_style) braces, where each brace begins on a new line.
2. We use four spaces of indentation (no tabs).
3. We use "_camelCase" private members and use "readonly" where possible.
4. We avoid `this.` unless absolutely necessary.
5. We always specify the visibility, even if it's the default (i.e.
   `private string _foo` not `string _foo`).
6. Namespace imports should be specified at the top of the file, *outside* of
   `namespace` declarations and should be sorted alphabetically, with `System.
   namespaces at the top and blank lines between different top level groups.
7. Avoid more than one empty line at any time. For example, do not have two
   blank lines between members of a type.
8. Avoid spurious free spaces.
   For example avoid `if (someVar == 0)...`, where the dots mark the spurious free spaces.
   Consider enabling "View White Space (Ctrl+E, S)" if using Visual Studio, to aid detection.
9. If a file happens to differ in style from these guidelines (e.g. private members are named `m_member`
   rather than `_member`), the existing style in that file takes precedence.
   There is an ongoing discussion on which cases to leave as-is and which to just change over to the
   standard style guidelines. Once the discussion concludes, this particular guideline will change
   accordingly.

### Example File:

``ObservableLinkedList`1.cs:``

```C#
using System;
using System.Collections;
using System.Collections.Generic;
using System.Collections.Specialized;
using System.ComponentModel;
using System.Diagnostics;

using Microsoft.Win32;

namespace System.Collections.Generic
{
    public partial class ObservableLinkedList<T> : INotifyCollectionChanged, INotifyPropertyChanged
    {
        private ObservableLinkedListNode<T> _head;
        private int _count;

        public ObservableLinkedList(IEnumerable<T> items)
        {
            if (items == null)
                throw new ArgumentException("items");

            foreach (T item in items)
            {
                AddLast(item);
            }
        }

        public event NotifyCollectionChangedEventHandler CollectionChanged;

        public int Count
        {
            get { return _count; }
        }

        public ObservableLinkedListNode AddLast(T value) 
        {
            LinkedListNode<T> newNode = new LinkedListNode<T>(this, value);

            InsertNodeBefore(_head, node);
        }

        protected virtual void OnCollectionChanged(NotifyCollectionChangedEventArgs e)
        {
            var handler = CollectionChanged;
            if (handler != null)
            {
                handler(this, e);
            }
        }

        private void InsertNodeBefore(LinkedListNode<T> node, LinkedListNode<T> newNode)
        {
           ...
        }
        
        ...
    }
}
```

``ObservableLinkedList`1.ObservableLinkedListNode.cs:``

```C#
using System;

namespace System.Collections.Generics
{
    partial class ObservableLinkedList<T>
    {
        public class ObservableLinkedListNode
        {
            private readonly ObservableLinkedList<T> _parent;
            private readonly T _value;

            internal ObservableLinkedListNode(ObservableLinkedList<T> parent, T value)
            {
                Debug.Assert(parent != null);

                _parent = parent;
                _value = value;
            }
            
            public T Value
            {
               get { return _value; }
            }
        }

        ...
    }
}
```
### Commits 
Please format commit messages as follows (based on this [excellent post](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)):

```
Summarize change in 50 characters or less

Provide more detail after the first line. Leave one blank line below the
summary and wrap all lines at 72 characters or less.

If the change fixes an issue, leave another blank line after the final 
paragraphand indicate which issue is fixed in the specific format below.

Fix #42
```

Also do your best to factor commits appropriately, i.e not too large with unrelated
things in the same commit, and not too small with the same small change applied N
times in N different commits. If there was some accidental reformatting or whitespace
changes during the course of your commits, please rebase them away before submitting
the PR.

### Issues
You do not need to file an issue for trivial changes (e.g. typo fixes). Just send us
a PR if it's tiny.

If an issue is complex, consider filing it and giving us time to respond before sending a
corresponding PR. If you want to work on the issue, just let it be known on the issue thread. Giving
us a chance to review the issue will help save time. For example, we might let you know why existing
behavior can't be changed or about particular implementation constraints you need to keep in mind, etc.

Don't feel obliged to match every issue with a PR. Simply filing issues for problems you
encounter is a great way to contribute too! 

### Style
We are striving to bring dotnet/corefx in to full conformance with the style guidelines
described above. We have built some automation for that and will work to bring everything
in line. In the meantime, please:

* **DO NOT** send PRs for style changes. We'd rather handle them holistically and they are tough
to review and merge.

* **DO** give priority to the current style of the project or file you're changing even if it
diverges from the general guidelines.

### Licensing
* **DO NOT** submit PRs that alter licensing related files or headers. If you believe there's a
problem with them, file an issue and let us take care of it.

### Compatibility
Please review the page about [Breaking Changes](Breaking Changes) before making changes.