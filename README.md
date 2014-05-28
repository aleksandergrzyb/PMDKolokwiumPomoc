PMDKolokwiumPomoc
=================
#MapReduce & CodeSnippets

Uruchamiamy MapReduce poleceniem:

	./bin/start-all.sh 
	
Uruchamiamy program:

	./bin/hadoop jar path_to_jar_file.jar input output

## Main

Dla każdego Mappera trzeba ustawić (przykład):

	job.setMapperClass(SQLJoinMapper.class);
	job.setMapOutputKeyClass(Text.class);
	job.setMapOutputValueClass(Text.class);
	
Dla każdego Reducera trzeba ustawić (przykład):

	job.setReducerClass(SQLJoinReducer.class);
	job.setOutputKeyClass(Text.class);
	job.setOutputValueClass(IntWritable.class);
	
**JEDEN JOB TO JEDEN MAPPER I REDUCER**

Jeżeli program wymaga by stworzyć więcej niż jedną parę Mappera i Reducera trzeba stworzyć drugiego Joba. Wtedy dane wejściowe przechowujemy w folderze tymczasowym, czyli (przykład):

	FileInputFormat.addInputPath(job, new Path(otherArgs[0] + "/sales.in"));
	FileInputFormat.addInputPath(job, new Path(otherArgs[0] + "/dates.in"));
	FileOutputFormat.setOutputPath(job, new Path(otherArgs[1] + "_tmp"));
	
Dla Joba 2:

	FileInputFormat.addInputPath(job2, new Path(otherArgs[1] + "_tmp"));
	FileOutputFormat.setOutputPath(job2, new Path(otherArgs[1]));

Przekazywanie parametru z main() do mappera. Najpierw przekazujemy w main:

	conf.set("NAZWA_ZMIENNEJ", otherArgs[2]);
	
Potem odczytujemy ten tekst w mapperze:

	tekst = context.getConfiguration("NAZWA ZMIENNEJ")
	
Powyższy tekst wykonujemy w funkcji setup()

	protected void setup(Context context) throws IOException, InterruptedException {
		};
	
## Mapper
	
Tworzenie zmiennej ONE:

	private final static IntWritable ONE = new IntWritable(1);
	
Tworzenie zmiennej Text:

	private Text word = new Text();
	
Przejście przez wszystkie słowa w wierszu:

	String line = value.toString();
	StringTokenizer itr = new StringTokenizer(line);
	while (itr.hasMoreTokens()) {
		word.set(itr.nextToken());
		contex.write(word, ONE);
	}

Przydatna funkcja do odczytania nazwy pliku:

	protected static String getInputFileName(Context context) {
		FileSplit fs = (FileSplit) context.getInputSplit();
		return fs.getPath().getName();
	}

## Reducer

Przydatne przy iterowaniu typu Iterable:

	while (values.iterator().hasNext()) {
		values.iterator().next().get(); // Odczytanie wartości
	}	



