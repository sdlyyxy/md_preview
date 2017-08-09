#!/usr/bin/python
import sys
import os


def usage():
    print('Usage: md <filename>')


def fileError():
    print('File error')


inputFile = ''
if(len(sys.argv) > 1):
    inputFile = sys.argv[1]
else:
    usage()
    exit()
try:
    f = open(inputFile, 'r')
    f.close()
except:
    fileError()
    exit()

try:
    os.mkdir('/tmp/md_preview')
except:
    print('hehe')
tmpPath = '/tmp/md_preview/%s.tmp' % inputFile
htmlPath = '/tmp/md_preview/%s.html' % inputFile
os.system("cmark %s>%s" % (inputFile, tmpPath))
f = open(htmlPath, 'w')
tmpf = open(tmpPath, 'r')
f.write('''
        <!doctype html>
        <html>
        <head>
        <meta charset='utf8'>
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <link rel="stylesheet" href="/home/sdlyyxy/project/md_preview/github-markdown.css">
        <link href="http://cdn.bootcss.com/highlight.js/8.0/styles/github.min.css" rel="stylesheet">  
        <script src="http://cdn.bootcss.com/highlight.js/8.0/highlight.min.js"></script>  
        <script >hljs.initHighlightingOnLoad();</script>  
        <style>
            .markdown-body {
            box-sizing: border-box;
            min-width: 200px;
            max-width: 980px;
            margin: 0 auto;
            padding: 45px;
            }
            @media (max-width: 767px) {
                .markdown-body {
                     padding: 15px;
                }
            }
            </style>
        </head>
        <body>
            <article class="markdown-body">
            %s
            </article>
        </body>
        </html>
''' % tmpf.read())
f.close()
tmpf.close()
os.system('firefox %s' % htmlPath)
