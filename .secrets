#!/usr/bin/env python
import sys
import os
import os.path
from whoosh import index
from whoosh import highlight
from whoosh.fields import ID, TEXT, Schema
from whoosh.reading import TermNotFound
from whoosh.qparser import QueryParser

SCHEMA = Schema(filename=ID(unique=True, stored=True),
                text_filename=ID(unique=True, stored=True),
                content=TEXT(),
                )


def get_or_create_index(index_dir):
    if not os.path.exists(index_dir):
        os.mkdir(index_dir)

    if index.exists_in(index_dir):
        return index.open_dir(index_dir)
    else:
        return full_index(index_dir)


def full_index(index_dir):
    idx = index.create_in(index_dir, SCHEMA)
    writer = idx.writer()
    _, _, pdf_files = next(os.walk('pdfs'))
    _, _, text_files = next(os.walk('text'))
    pdf_files.sort()
    text_files.sort()

    datas = []

    for pdf, text in zip(pdf_files, text_files):
        with open(os.path.join('text', text), 'r') as content:
            data = {'filename': os.path.join('pdfs', pdf),
                    'text_filename': os.path.join('text', text),
                    'content': content.read(),
                    }
            datas.append(data)

    for data in datas:
        writer.add_document(**data)

    writer.commit()
    return idx


class BashFormatter(highlight.Formatter):
    def format_token(self, text, token, replace=False):
        tokentext = highlight.get_text(text, token, replace)
        return "{}{}{}".format(self.white, tokentext, self.clear)

    white = "\033[1;37m"
    red = "\033[0;31m"
    blue = "\033[1;34m"
    clear = "\033[0m"


def search(user_query, index_dir):
    idx = get_or_create_index(index_dir)

    qp = QueryParser('content', schema=idx.schema)
    query = qp.parse(user_query)

    bash = BashFormatter()
    with idx.searcher() as searcher:
        try:
            results = searcher.search(query)
        except TermNotFound:
            results = []
        print("{}{} files matched:{}".format(
            bash.blue, len(results), bash.clear))
        results.formatter = bash
        results.fragmenter.maxchars = 100
        for hit in results:
            filecontents = ''
            with open(hit['text_filename']) as file_:
                filecontents = file_.read()
                highlight = (hit
                             .highlights("content", text=filecontents, top=2)
                             .replace("\n", " ").strip())
            print("  * {}{}{} - \n\t{}...".format(
                bash.red, hit['filename'], bash.clear, highlight))


if __name__ == '__main__':
    try:
        query = sys.argv[1]
    except IndexError:
        print('you need to provide a query. assuming "chicken" query.')
        search('chicken', 'index_dir')
    else:
        search(query, 'index_dir')
