# Extend Android Java Native Interface

## Build
include_directories(
        EAJNI/include/
)

aux_source_directory(
        EAJNI
        SRC_LIST
)

add_library(
        eajni
        SHARED
        ${SRC_LIST}
)

find_library(
        log-lib
        log )

target_link_libraries(
        eajni
        ${log-lib} )

target_link_libraries(
        xxx-lib
        ${log-lib}
        eajni)

## Usage
#include <eajnis/AndroidJNI.h>

jint JNI_OnLoad(JavaVM *vm, void * /*reserved*/)
{
    AndroidJNI::init(vm);
    ...
}

JNIEnv *env = AndroidJNI::getJNIEnv();
void thread_run(void *arg)
{
    jobject user = (jobject) arg;
    JNIEnv *env = AndroidJNI::getJNIEnv();
    env->CallStaticVoidMethod(gClass, gPostEventMethod, user, array);
}

static void xxx_xxx_xxx_Xxx_xxx(JNIEnv *env, jobject thiz, jobject ref)
{
    user = env->NewGlobalRef(ref);
    AndroidJNI::createJavaThread("JNI-Thread-Name", thread_run, (void *)user);
}
