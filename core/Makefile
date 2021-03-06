PROJECT_SOURCEFILES = pubnub_pubsubapi.c pubnub_coreapi.c pubnub_ccore_pubsub.c pubnub_ccore.c pubnub_netcore.c pubnub_alloc_static.c pubnub_assert_std.c pubnub_json_parse.c pubnub_keep_alive.c pubnub_helper.c

all: pubnub_proxy_unittest pubnub_timer_list_unittest unittest #generate_report

#generate_report:
#	gcovr -r . --html --html-details -o coverage.html

ifndef RECEIVE_GZIP_RESPONSE
RECEIVE_GZIP_RESPONSE = 1
endif

ifeq ($(RECEIVE_GZIP_RESPONSE), 1)
PROJECT_SOURCEFILES += ../lib/miniz/miniz_tinfl.c pbgzip_decompress.c
endif

CFLAGS +=-g -D PUBNUB_ADVANCED_KEEP_ALIVE=1 -D PUBNUB_LOG_LEVEL=PUBNUB_LOG_LEVEL_TRACE -D PUBNUB_DYNAMIC_REPLY_BUFFER=1 -D PUBNUB_RECEIVE_GZIP_RESPONSE=$(RECEIVE_GZIP_RESPONSE) -I. -I../ -I test -I../lib/base64 -I../lib/md5 -I../lib/miniz

unittest: $(PROJECT_SOURCEFILES) pubnub_core_unit_test.c
	gcc -o pubnub_core_unit_test.so -shared $(CFLAGS) -D PUBNUB_ORIGIN_SETTABLE=1 -Wall -fprofile-arcs -ftest-coverage -fPIC $(PROJECT_SOURCEFILES) pubnub_core_unit_test.c -lcgreen -lm
#	gcc -o pubnub_core_unit_testo  $(CFLAGS) -Wall -fprofile-arcs -ftest-coverage $(PROJECT_SOURCEFILES) pubnub_core_unit_test.c -lcgreen -lm
	valgrind --quiet cgreen-runner ./pubnub_core_unit_test.so
	gcovr -r . --html --html-details -o coverage.html

TIMER_LIST_SOURCEFILES = pubnub_alloc_static.c pubnub_assert_std.c pubnub_timers.c

pubnub_timer_list_unittest: pubnub_timer_list.c pubnub_timer_list_unit_test.c
	gcc -o pubnub_timer_list_unit_test.so -shared $(CFLAGS) -D PUBNUB_CALLBACK_API -D PUBNUB_ASSERT_LEVEL_NONE -Wall -fprofile-arcs -ftest-coverage -fPIC $(TIMER_LIST_SOURCEFILES) pubnub_timer_list.c pubnub_timer_list_unit_test.c -lcgreen -lm
#	gcc -o pubnub_timer_list_unit_testo  $(CFLAGS) -D PUBNUB_CALLBACK_API -Wall -fprofile-arcs -ftest-coverage $(TIMER_LIST_SOURCEFILES) pubnub_timer_list.c pubnub_timer_list_unit_test.c -lcgreen -lm
	valgrind --quiet cgreen-runner ./pubnub_timer_list_unit_test.so
	gcovr -r . --html --html-details -o coverage.html

PROXY_PROJECT_SOURCEFILES = pubnub_proxy_core.c pubnub_proxy.c pbhttp_digest.c pbntlm_core.c pbntlm_packer_std.c pubnub_generate_uuid_v4_random_std.c ../lib/base64/pbbase64.c ../lib/md5/md5.c

pubnub_proxy_unittest: $(PROJECT_SOURCEFILES) $(PROXY_PROJECT_SOURCEFILES) pubnub_proxy_unit_test.c
	gcc -o pubnub_proxy_unit_test.so -shared $(CFLAGS) -D PUBNUB_PROXY_API=1 -Wall -fprofile-arcs -ftest-coverage -fPIC $(PROJECT_SOURCEFILES) $(PROXY_PROJECT_SOURCEFILES) pubnub_proxy_unit_test.c -lcgreen -lm
#	gcc -o pubnub_proxy_unit_testo -D PUBNUB_PROXY_API=1 $(CFLAGS) -Wall -fprofile-arcs -ftest-coverage $(PROJECT_SOURCEFILES) pubnub_proxy_core.c pubnub_proxy.c pbhttp_digest.c ../lib/base64/pbbase64.c ../lib/md5/md5.c pubnub_proxy_unit_test.c -lcgreen -lm
	valgrind --quiet cgreen-runner ./pubnub_proxy_unit_test.so
	gcovr -r . --html --html-details -o coverage.html

clean:
	rm pubnub_core_unit_test.so pubnub_timer_list_unit_test.so pubnub_proxy_unit_test.so *.gcda *.gcno *.html
