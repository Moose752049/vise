add_subdirectory( tests )
add_subdirectory( train )

add_library( build_index build_index.cpp )
target_link_libraries( build_index
    ViseMessageQueue
    build_index_status.pb
    clst_centres
    dataset_v2
    embedder
    feat_getter
    image_util
    index_entry.pb
    index_entry_util
    par_queue
    protobuf_util
    proto_db_file
    proto_index
    ${fastann_LIBRARIES}
    ${Boost_LIBRARIES} )

PROTOBUF_GENERATE_CPP(build_index_status.pb.cpp build_index_status.pb.h build_index_status.proto)
add_library( build_index_status.pb ${build_index_status.pb.cpp} )
target_link_libraries( build_index_status.pb ${PROTOBUF_LIBRARIES} )

#add_executable( compute_index_v2 compute_index_v2.cpp )
#target_link_libraries( compute_index_v2
#    ViseMessageQueue
#    build_index
#    dataset_v2
#    embedder
#    feat_standard
#    hamming_embedder
#    mpi_queue
#    train_assign
#    train_descs
#    train_hamming
#    ${Boost_LIBRARIES} )

add_library( daat daat.cpp )
target_link_libraries( daat index_entry_util index_entry.pb uniq_entries )

PROTOBUF_GENERATE_CPP(index_entry.pb.cpp index_entry.pb.h index_entry.proto)
add_library( index_entry.pb ${index_entry.pb.cpp} )
target_link_libraries( index_entry.pb ${PROTOBUF_LIBRARIES} )

add_library( index_entry_util index_entry_util.cpp )
target_link_libraries( index_entry_util
    embedder
    index_entry.pb
    protobuf_util
    ${Boost_LIBRARIES} )

add_library( proto_db proto_db.cpp )
target_link_libraries( proto_db slow_construction ${Boost_LIBRARIES} )

add_library( proto_db_file proto_db_file.cpp )
target_link_libraries( proto_db_file proto_db_header.pb ${Boost_LIBRARIES} )

PROTOBUF_GENERATE_CPP(proto_db_header.pb.cpp proto_db_header.pb.h proto_db_header.proto)
add_library( proto_db_header.pb ${proto_db_header.pb.cpp} )
target_link_libraries( proto_db_header.pb ${PROTOBUF_LIBRARIES} ) # added by @Abhishek to support compilation in Mac

add_library( proto_index proto_index.cpp )
target_link_libraries( proto_index
    index_entry.pb
    index_entry_util
    proto_db
    slow_construction
    thread_queue
    uniq_entries
    ${Boost_LIBRARIES} )

add_library( proto_index_cached proto_index_cached.cpp )
target_link_libraries( proto_index_cached
    proto_index
    ${Boost_LIBRARIES} )

add_library( proto_index_limit proto_index_limit.cpp )
target_link_libraries( proto_index_limit
    proto_index
    protobuf_util )

add_library( uniq_entries uniq_entries.cpp )
target_link_libraries( uniq_entries index_entry.pb ${Boost_LIBRARIES} ) # added by @Abhishek to support compilation in Mac
