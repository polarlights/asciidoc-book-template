require 'open3'

SOURCE_FILES = ['index.adoc']
SOURCE_DIR = 'src/docs/asciidoc'

COMMON_ATTRIBUTES = {
    'source-highlighter' => 'rouge',
    'epub3-stylesdir'    => './styles/epub',
    'imagesdir'          => 'images',
    'toc'                => 'left',
    'icons'              => 'font',
    'sectanchors'        => '',
    'idprefix'           => '',
    'idseparator'        => '-'
}

def exec_command(cmd, msg = '')
  puts cmd
  output_str, status = Open3.capture2("#{cmd} 2>&1 > /dev/null")
  if status.success?
    return true
  else
    raise msg
  end
end

def convert_to_file(attrs, output_dir, backend, require_files = [])
  attributes = attrs.to_a.map{|e| '-a ' + e.join('=') }.join(' ')
  _require_files = require_files.map { |file| "-r #{file}" }.join(' ')
  SOURCE_FILES.each do |source_file|
    cmd = "bundle exec asciidoctor #{_require_files} -R #{SOURCE_DIR} -b #{backend} -D #{output_dir} #{attributes} #{[SOURCE_DIR, SOURCE_FILES].join('/')}"
    exec_command(cmd, "exporting #{source_file}")
  end
end

def generate_html
  attrs = COMMON_ATTRIBUTES
  output = 'build/html'
  backend = 'html5'
  convert_to_file(attrs, output, backend)
end

def generate_epub
  attrs = COMMON_ATTRIBUTES.merge({
  })
  output = 'build/epub'
  backend = 'epub3'
  convert_to_file(attrs, output, backend, ['asciidoctor-epub3'])
end

def generate_pdf
  attrs = COMMON_ATTRIBUTES.merge({
    'pdfmarks' => '',
    'pdf-fontsdir' => "#{SOURCE_DIR}/styles/pdf/fonts",
    'pdf-stylesdir' => "#{SOURCE_DIR}/styles/pdf",
    'pdf-style' => 'infoq-screen',
     'scripts' => 'cjk'
    })
  files = ['asciidoctor-pdf', './src/main/ruby/asciidoctor-pdf-extensions.rb']
  output = 'build'
  backend = 'pdf'

  convert_to_file(attrs, output, backend, files)
end


task :html do
  generate_html
end

task :pdf do
  generate_pdf
end

task :epub do
  generate_epub
end

task :all do
  generate_html
  generate_epub
  generate_pdf
end
