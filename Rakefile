## Changeable settings
@root = 'report'
@standalone = 'standalone'
@poster = 'poster'
@slides = 'slides'
@compiler = 'xelatex'
@pdflatex = 'pdflatex'
@options = '-shell-escape -interaction=nonstopmode'
@bib = 'bibtex'

task :default => [:compile]

task :slide do
  puts "Compiling slide.."
  compileStr = "#{@pdflatex} #{@options} #{@slides}"
  puts "Compiling twice"
  system(compileStr + ' > /dev/null')
  system(compileStr + ' > /dev/null')
  task(:find_errors).invoke("#{@slides}.log")
  task(:clean).invoke
  puts "Done"
end

task :standalone do
  puts "Compiling standalone.."
  compileStr = "#{@pdflatex} #{@options} #{@standalone}"
  puts "Compiling once"
  system(compileStr + ' > /dev/null')
  task(:find_errors).invoke("#{@standalone}.log")
  task(:clean).invoke
  puts "Done"
end

task :poster do
  puts "Compiling poster.."
  compileStr = "#{@pdflatex} #{@options} #{@poster}"
  puts "Compiling twice"
  system(compileStr + ' > /dev/null')
  system(compileStr + ' > /dev/null')
  task(:find_errors).invoke("#{@poster}.log")
  task(:clean).invoke
  puts "Done"
end

task :compile do
  puts "Compiling #{@root}..."
  compileStr = '%s %s %s' % [@compiler, @options, @root]
  bibtexStr = '%s %s' % [@bib, @root]

  puts "#{@compiler} 1 of 3"
  system(compileStr + ' > /dev/null')
  puts "#{@bib} once"
  system(bibtexStr + ' > /dev/null')
  puts "#{@compiler} 2 of 3"
  system(compileStr + ' > /dev/null')
  puts "#{@compiler} 3 of 3"
  system(compileStr + ' > output')

  task(:find_errors).invoke('output')
  task(:clean).invoke
end

task :clean do
  puts "Cleaning..."
  rm = 'rm -rf'
  tmp_all = %w[aux log nav snm tdo pyg lof lot fls fdb_latexmk toc vrb bbl bcf blg out bib.bak thm run.xml]
    .map{|f| '*.' + f}
    .join(' ')
  tmp_full = %W[output].join(' ')

  system("#{rm} #{tmp_all} #{tmp_full}")
  system('rm -f **/*.aux')

  unless @error
    puts "Success!"
  end
end

task :find_errors, :file do |t, args|
  @errors = ''

  File.open(args[:file], 'r').each do |line|
    @errors += line if line.start_with?('!')
  end

  if @errors.length > 0
    File.open(args[:file], 'r').each{|l| puts l}
    puts '--- ERROR COMPILING, CHECK ABOVE LOG ---'
    puts '--- SEARCH FOR THE FOLLOWING ERRORS  ---'
    puts @errors
    puts '--- DONE ---'
  end
end
